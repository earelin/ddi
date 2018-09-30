# Drupal docker infrastructure
Drupal infrastructure based on Docker.

Folder | Description
--- | ---
/ca | Root CA
/aws | AWS Cloudformation templates
/conf | Configuration files for docker-compose execution
/conf/nginx/certs | NGINX https certificates
/conf/nginx/vhosts | NGINX virtual hosts configuration
/var | Services data storage
/www | Webserver documents folder

## Services

Container name | Exposed ports | Service provided
--- | --- | ---
ddi-d7-cli | 2223 ssh | CLI with tools for Drupal 7
ddi-d8-cli | 2222 ssh | CLI with tools for Drupal 8
ddi-db | 3306 mysql | MySQL 5.7
ddi-memcached | | Memcached
ddi-php5 | 9002 xdebug | PHP-FPM pool. PHP version 5.6
ddi-php7 | 9001 xdebug | PHP-FPM pool. PHP version 7.1
ddi-redis | | Redis
ddi-web | 80 http, 443 https | NGINX webserver

# Installing

Clone this repository. Containers have to be build and launch using docker-compose.

```bash
$ docker-compose build
$ docker-compose up -d
```

## Domains and certificates

Nginx is configured to have a virtual host per website. Every virtual host must have an domain host name. On the NGINX virtual host configuration folder there are 2 configuration examples drupal7.conf and drupal8.conf. The system support https to avoid errors on modern browsers. NGINX is configured by default with a certificate for all *.ddi.dev domains. So that drupal8.conf host is configured for the domain name drupal8.ddi.dev. More virtual host based on the *.ddi.dev pattern can be created with the same configuration. Those domain names has to be added to the /etc/hosts configuration file on the computer running Docker.

```bash
127.0.0.1 drupal7.ddi.com
127.0.0.1 drupal8.ddi.com
```

The root certificate (ca/ca.pem) has to be installed on the browsers.

# Usage

There are two containers configured with tools for building and managing Drupal websites:

Container name | Tools
--- | ---
ddi-d7-cli | Composer, Drush 8
ddi-d8-cli | Composer, Drush 9

The two command line interfaces can be accessed by ssh, port 2222 for Drupal 8 and 2223 for Drupal 7.
There is a preconfigured user for the containers:

Username | Password
--- | ---
developer | dev

Database server credentials:

Username | Password
--- | ---
root | dev

Websites should be created on the www folder. That folder is accesible from the CLI containers on the path /www and from the computer executing docker from the www folder on the root of this repository.
