# Nginx Reverse Proxy

Nginx will be configured to allow PHP scripts (for use only with continuous
deployment for the frontend) and have the access logs set to json format
as described [here](https://medium.com/bolt-labs/using-json-for-nginx-log-format-793743064fc4)
which will make tailing Nginx logs less painful. We'll just store the path,
status code and time and dump them into the postgres database in one bulk
transaction every minute. One of the database cleanup tasks will be to delete
requests older than 1 month.

This repository will be responsible for the access log tailing script and
possibly some config files.

We'll use LetsEncrypt for the SSL cert which is so automated that it probably
won't be worth init scripts.
