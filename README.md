# webfortress
{{ EPITECH }}

## Goals

* Create a website with a database
* Secure website access
* Secure website and database communication
* Keep backup of database
* Monitor website
* Limited and secure monitoring access
* Use docker and docker-compose

## Architecture

Show doc folder

## Techno

* Nginx as reverse proxy
* Wordpress with Mysql for website and database
* Prometheus for monitoring
* openssl for certificate generation

## Wordpress

We extract and popul `wp-config.php` config file. This file contain database connection information (login + password) and force SSL connection to Mysql. Due to login + password configuration **you have to update this file with you .env config and remove this file from version control**.

## Mysql

We keep Mysql data in a volume. Moreover when docker image is build we inject database certificates config for SSL connection.
**You have to create your own certificate with letsencrypt, openssl, ....**.

## Monitoring

Monitoring it's really important. We add monitoring with a wordpress configuration and with Mysql tools (you can found in commit code for go back on this configuration). There solutions don't allow SSL connection. But, as this subject is about security, we prefered remove database monitoring and keep SSL as mandatory.
In the real word, we have to found a solution. Maybe thanks to docker monitoring ?..

## Reverse Proxy

There are a reverse proxy in front of websites for provide HTTPS and another reverse proxy in front of the monitoring system for provide HTTPS + IP whitelist.
**You have to change IP allow in `monitoring/proxy_ssl.conf`**.

## Getting started

* Provide certificat for RP + database (`monitoring`, `mysql`, `website` directories).
* Change IP whitelist in `monitoring/proxy_ssl.conf`.
* Change in `docker-compose-*` volumes paths.
* Copy/path `.env.example` to `.env` and provide real value. Don't forget to spread change in config files. => Yes we have to improve this.

## Other way

We also make a POC with Azure.

We create Mysql Database in DBAAS.

We create Azure Container Service with a Wordpress. We plug Azure Files on it for conf and storage. And, we connect it to the database.

We plug a Application Gateway in front of the ACS for WAF protection.

We apply IP filtering rules on database and ACS services.

We plug a AppInsigth on each break for monitoring, alerting.


The Cloud way is better but more expensive. 
* 90$/month for ACS + appInsight.
* 60$/month for a basic database.

Without network cost and in the minimal prod configuration.

If you take server in OVH, you have to buy at least 3 server for docker swarm for 60euros/month. But you have to add some security for data backup (system like Gluster).

A other advantage of the "on premise" way, is if you want to add a service, you can simply and quickly test in local.
