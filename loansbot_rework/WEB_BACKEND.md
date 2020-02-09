# Web Backend

This is using python with the FastAPI+uvicorn framework for serving requests,
and psycopg2+PyPika for the connection with the database.

This is responsible for serving requests from the /api/ directory on the
website.

Expected to be challenging migrations:

- `/api/loans.php` and `/api/users.php` are complicated endpoints. We're going
  to reimplement them (although the url will be misleading) and deprecate them
  in favor of GraphQL

Deprecation warnings:

- The `.php` suffixes will be supported and deprecated. An automatic gating
  strategy will be used for these aliases:
  - Starting 03/01/2021, these requests will fail with an explanation of
    deprecation for all requests on the first day of the month, then
    resume normal operation for the remainder of the month.
  - Starting 06/01/2021, these requests will fail unless the request is
    authorized. For authorized connections, a reddit message is sent to
    the account when a request is made to this endpoint, but no more than
    once per week.
  - Starting 09/01/2021 these requests will return a 301 Moved Permanently
  - Starting 01/01/2022 these requests will fail with a 404 Not Found
- The `/api/loans` and `/api/users` endpoints will be replicated and deprecated
  in favor of GraphQL
  - Starting 01/01/2022 this endpoint will fail for all non-authenticated
    callers on the first day of the month with an explanation of deprecation,
    and resume normal operations for the remainder of the month. Users which
    are authorized will be sent a reddit PM once on the first of each month
    instead. Both of these can be suppressed by setting
    "deprecation_warning_heard=true" as a query parameter.
  - Starting 06/01/2022 this endpoint will fail for all unauthenticated users
    unless they include a "deprecation_warning_heard=true" query parameter.
  - Starting 09/01/2022 this endpoint will fail for all users unless they
    include a "deprecation_warning_heard=true" query parameter.
  - Starting 01/01/2023 this endpoint will 404.
  - Starting 03/01/2023 this endpoint will be considered ready for reuse.
- The `/api/log` endpoint is only for moderators, but it will be moved
  to `/api/ops/log`.

Additional features/changes to the web backend:

- GitHub actions for automated testing
- AWS CodePipeline for continuous deployment
- FastAPI will be configured to serve docs under `/api/docs/swagger` and
  `/api/docs/redoc`. The OpenAPI schema will be available under
  `/api/docs/openapi.json`.
- GraphQL backend served under `/api/graphql/users` and `/api/graphql/loans`
  which will hopefully allow easier third-party integration.
- `/api/log/` will be moved to `/api/ops/log`, and there will be new endpoints
  for monitoring: `/api/ops/health`
