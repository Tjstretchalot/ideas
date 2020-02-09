# Reddit Proxy

This server will specifically be intended to accept from incoming queues,
process requests, and responds using the RPC pattern. It is built in
python and uses the requests library.

The messages have a special mapping to reddit API endpoints, so that if the
reddit API changes we can have a different internal deprecation timeline
than reddit (if we choose to deprecate instead of alias when reddit
deprecates/modifies). This, however, is not the main reason for this server.

This server is mainly used to fairly distribute our reddit API time between
our web backend and the actual LoansBot itself, as well as provide a single
location to perform the reasonably complicated back-off logic.

This server is not responsible for caching operations, such as deleted users.
