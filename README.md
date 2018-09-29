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
--- | --- | ---
ddi-d7-cli | 2223 ssh | CLI with tools for Drupal 7
ddi-d8-cli | 2222 ssh | CLI with tools for Drupal 8
ddi-db | 3306 mysql | MySQL 5.7
ddi-memcached | | Memcached
ddi-php5 | 9002 xdebug | PHP-FPM pool. PHP version 5.6
ddi-php7 | 9001 xdebug | PHP-FPM pool. PHP version 7.1
ddi-redis | | Redis
ddi-web | 80 http, 443 https | NGINX webserver
