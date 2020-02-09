# Logging

Logging is currently done to a flatfile which is also available via
/api/log.php. This was only possible because of the single-server
infrastructure. We will now instead leverage the larger Postgres server to
provide better searching, rollover, and automatically combining the logs from
various sources.

Logs will follow a schema roughly as follows:

- When did the event occur
- Which application
- Frame which called the log function (file + function + lineno)
- Level (trace/debug/info/warn/error)
- Message

This repository provides a python interface for logging.

The `/api/log.php` function will be replaced with `/api/ops/graphql/log`
which allows you to filter by timestamps, severity,
