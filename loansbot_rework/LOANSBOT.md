# LoansBot

The loansbot will be rewritten in python using pydantic, the RabbitMQ
client, and psycopg2 + PyPika.

The existing LoansBot implementation has the following advantages:

- Modular "summons" and a sophisticated query parsing and detection system
- Business logic fairly separated from database and external API calls
- Reddit API connections which aren't ridiculous
- File configuration
- Database schema creation and validation
- Response infos are editable via the database
- Robust error handling and error reporting

Having seen this bot in production, CPU usage is not going to be a serious
issue compared to the number of API calls. The most complicated thing by far
is ensuring the bot can handle errors and restarts smoothly - which is likely
to be easier in a dynamically typed language. Memory usage will still need to
be monitored as there won't be much to go around.

Many features either have to be reworked to support the new infrastructure or
have adequate replacements:

- Query parsing and detection: unfortunately AFAIK there still aren't any great
  ways to do this if a regex fails. Fortunately the logic is fairly language
  agnostic.
- Database calls can be redone using a query builder. I'm still not a fan of
  ORMs, but query builders are definitely waaay better than the current
  approach. There's so much repetition in using the JDBC directly that it's
  by far the most unwieldy part of the application (although it is fairly
  stable)
- The reddit API calls will be moved to a dedicated reddit communicator in
  order to streamline requests without exceeding our quota and thus will
  be reworked anyway.
- File configuration can be done with ConfigParser which looks good and is a
  built-in. Still super verbose.
- Database schema creation and validation is being moved to a repo dedicated
  to database maintanence scripts (creation, backups, etc)
- Response infos editable via the database: we can use the same technique.
- Error handling dynamic will change significantly due to the reddit proxy.

The following new features will be added upon rework:

- GitHub actions for automated testing and code coverage
- AWS CodePipeline for continuous deployment
- Using a dedicated reddit proxy application to fairly distribute reddit
  API time with the web backend. This will also reduce the sleep-clutter
  throughout the application.
- Using a true message queue for communicating with the web backend rather
  than abusing the database. The database will only be used for things we
  actually want to hold a record for. This means we send out account
  claim requests / notifications quicker and they don't need to clutter
  the loansbot code.
- Improve the handling for deleted users and moderators. Currently adding and
  removing moderators can only be done by modifying the database, and deleted
  user handling is automatic but requires a lot of replicated logic as it was
  added late.
