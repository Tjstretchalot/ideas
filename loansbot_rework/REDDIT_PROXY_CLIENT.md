# Reddit Proxy Client

The reddit proxy is communicated via the AMQP server, but both the actual
LoansBot and the web-backend will communicate with it. Rather than duplicating
that logic there will be a shared repository for interacting with those queues.

- This has automated testing and code coverage reports using GitHub actions
- This triggers tests and continuous deployment for the web backend and
  the loansbot using GitHub Actions
