# LoansBot Rework

New infrastructure layout: ![Picture for Infra](infra.png)

- Nginx reverse proxy: a1.medium (3yr reserved, no upfront, dedicated, convertible): $10.37/mo
- Fastapi: t3.micro (3yr reserved, no upfront, not dedicated, convertible): $3.80/mo
- Postgres: m5a.large (3yr reserved, no upfront, dedicated, convertible): $35.77/mo
- LoansBot + RabbitMQ + Reddit Proxy: a1.medium (3yr reserved, no upfront, not dedicated, convertible): $10.37/mo

Backups: S3 Standard - Infrequent Access

- Daily via cronjob
- Assuming ~128mb/backup (current: 76mb)
- 365 day lifetime
- ((128(mb/backup) / 1024(mb/gb)) * 365 backups) = 46.08 gb
- 46.08gb * 0.0125gb/mo = ~0.58$ / mo

Net total price (monthly): $60.89/mo

---

The following repositories:

- LoansBot/reverse-proxy
- LoansBot/web-frontend
- LoansBot/web-backend
- LoansBot/database
- LoansBot/amqp
- LoansBot/reddit-proxy