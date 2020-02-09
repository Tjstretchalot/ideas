# LoansBot Rework

New infrastructure layout: ![Picture for Infra](infra.png)

- Nginx reverse proxy + frontend (React): a1.medium (3yr reserved, no upfront, convertible): $9.28/mo
- Website backend (FastAPI): t3a.small (3yr reserved, no upfront, convertible): $6.87/mo
- Postgres: m5a.large (3yr reserved, no upfront, convertible): $31.39/mo
- LoansBot + RabbitMQ + Reddit Proxy: a1.medium (3yr reserved, no upfront, convertible): $9.28/mo

Backups: S3 Standard - Infrequent Access

- Daily via cronjob
- Assuming ~128mb/backup (current: 76mb)
- 365 day lifetime
- ((128(mb/backup) / 1024(mb/gb)) * 365 backups) = 46.08 gb
- 46.08gb * 0.0125gb/mo = ~0.58$ / mo

Net total price (monthly): $57.40/mo

---

Possible scaling: The FastAPI is a possible bottleneck depending on how many
API consumers there are, especially if they are using GraphQL. Since there
aren't many users, it's not immediately clear if horizontal scaling will be
as cost-effective as vertical scaling (or effective at all). However, it might
be helpful to move the bot-only endpoints to a separate instance before
vertically scaling.

---

The following repositories:

- LoansBot/reverse-proxy
- LoansBot/web-frontend
- LoansBot/web-backend
- LoansBot/database
- LoansBot/amqp
- LoansBot/reddit-proxy
