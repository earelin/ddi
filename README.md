# Drupal8 infrastructure
Drupal 8 infrastructure based on Docker.

File structure

Folder | Description
--- | ---
/aws | AWS Cloudformation templates
/conf | Configuration files for docker-compose execution
/var | Services data storage
/www | Webserver documents folder

## Services

Container name | Service provided
--- | ---
d8i-web | NGINX webserver
d8i-php5 | PHP-FPM pool. PHP version 5.6
d8i-php7 | PHP-FPM pool. PHP version 7.1
d8i-db | MySQL 5.7
d8i-memcached | Memcached
d8i-d7-cli | SSH service to provide CLI access for Drupal 7
d8i-d8-cli | SSH service to provide CLI access for Drupal 8
