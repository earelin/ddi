# Drupal8 infrastructure
Drupal 8 infrastructure based on Docker.

Folder | Description
--- | ---
/aws | AWS Cloudformation templates
/conf | Configuration files for docker-compose execution
/conf/nginx/certs | NGINX https certificates
/conf/nginx/vhosts | NGINX virtual hosts configuration
/var | Services data storage
/www | Webserver documents folder

## Services

Container name | Exposed ports | Service provided
--- | ---
d8i-d7-cli | 2223 ssh | CLI with tools for Drupal 7
d8i-d8-cli | 2222 ssh | CLI with tools for Drupal 8
d8i-db | 3306 mysql | MySQL 5.7
d8i-memcached | | Memcached
d8i-php5 | 9002 xdebug | PHP-FPM pool. PHP version 5.6
d8i-php7 | 9001 xdebug | PHP-FPM pool. PHP version 7.1
d8i-redis | | Redis
d8i-web | 80 http, 443 https | NGINX webserver

## Usage
