# Reddit Proxy Client

The reddit proxy is communicated via the AMQP server, but both the actual
LoansBot and the web-backend will communicate with it. Rather than duplicating
that logic there will be a shared repository for interacting with those queues.

- This has automated testing and code coverage reports using GitHub actions
- Commits to master triggers creating an empty commit in the web backend and
  the LoansBot master branches. Commits to develop triggers an empty commit
  in the web backend and LoansBot develop branches. This essentially ensures
  that changes propagate through automated testing and CI/CD without a ton
  of repetition.
