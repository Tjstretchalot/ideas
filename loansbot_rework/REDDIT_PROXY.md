# Reddit Proxy

This server will specifically be intended to accept from incoming queues,
process requests, and responds using a response queue. It is built in
python and uses the requests library.

The messages have a special mapping to reddit API endpoints, so that if the
reddit API changes we can have a different internal deprecation timeline
than reddit (if we choose to deprecate instead of alias when reddit
deprecates/modifies). This, however, is not the main reason for this server.

This server is mainly used to fairly distribute our reddit API time between
our web backend and the actual LoansBot itself, as well as provide a single
location to perform the reasonably complicated back-off logic.

This server is not responsible for caching operations, such as deleted users.

Messages will generally be in the format

```json
{
    "type": "request_type",
    "response_queue": "my_response_queue",
    "uuid": "7c07f3c0-f62c-43a0-badc-ce89869547e2",
    "version_utc_seconds": 1581255042.707,
    "sent_at": 1581255043.000,
    "args": {},
    "style": {
        "2xx": { "operation": "copy" },
        "4xx": { "operation": "failure" },
        "5xx": { "operation": "retry" }
    }
}
```

The `request_type` typically corresponds to an alias for a reddit endpoint
(i.e., `user_listing`). Request types much match one of the fixed set of keys
known by the reddit proxy. Request types which are prefixed with an underscore
don't actually correspond to reddit API calls and are instead for configuration
and monitoring purposes - these only give `success` and `failure` responses.

The `style` argument may be omitted to get the above behavior. The response
queue may be omitted for no response. The valid operations are as follows:

- `copy`: The response queue object will include the status code and body. By
  default the response will be interpreted as json and then re-encoded. This
  may be suppressed with `"raw": true`. The only exception is for blank
  responses, which return a blank string as the body. If an error occurs
  processing the body and `"raw"` is not set to true, it will be treated as
  failure.
- `success`: The response queue object will just indicate success, without the
  status code or body.
- `failure`: The response queue object will just indicate failure, without the
  status code or body.
- `retry`: The operation will be requeued. By default, the operation will be
  retried either until success, a newer version of the application connects
  to the given response queue, or the response is explicitly cleared. The
  operation will be retried even when a newer application connects if
  `"ignore_version": true` is specified.

There are no other valid keys for the `style` dict. All other status codes will
be treated as `failure`. The status code `429 Too Many Requests` is handled
specially and will always be treated as `retry` (as the proxy should never
allow this to happen). Similarly with `401 Unauthorized`.

By default all operations are logged. `copy` and `success` default to `TRACE`
level, whereas `failure` and `retry` default to `WARN`. Logging for a request
for a given status code can be silenced by setting `"silent": true`. For
example:

```json
"style": {
    "2xx": { "operation": "copy", "silent": true }
}
```

The level of logging can be set with the `"level"` key, for example
`"level": "info"`.

When an application starts and connects to the reddit proxy it needs to get
the current time in seconds since the utc epoch and pass that along to stop old
retry events. This is typically done with the `_connect` request type:

```json
{
    "type": "_connect",
    "response_queue": "my_response_queue",
    "uuid": "f7fc0c8f-bdb2-4310-9fd8-47bcf599cedd",
    "version_utc_seconds": 1581255453.311,
    "sent_at": 1581255454.050,
}
```

It's still possible there are some remaining stale responses in the response
queue; the client should filter by the version as well.

Responses generally take the following format:

```json
{
    "type": "copy",
    "uuid": "7c07f3c0-f62c-43a0-badc-ce89869547e2",
    "version_utc_seconds": 1581255453.311,
    "handled_at": 1581255525.050,
    "info": {
        "status": 201,
        "body": ""
    }
}
```

For `success` and `failure` responses the `"info"` key is omitted.
