FROM php:5.6-cli

RUN set -x \
	&& buildDeps=" \
        automake \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libltdl-dev \
        libmagickwand-dev \
        libmemcached-dev \
        libpng-dev \
        libssh2-1-dev \
        libtool \
        libxml2-dev \
        libyaml-dev \
	" \
    && runtimeDeps=" \
        ca-certificates \
        ctags \
        git \
        imagemagick \
        libfreetype6 \
        libjpeg62-turbo \
        libmemcached11 \
        libmemcachedutil2 \
        libpng16-16 \
        libssh2-1 \
        libxml2 \
        libyaml-0-2 \
        mysql-client \
        ssh \
        unzip \
        vim \
        wget \
    " \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get dist-upgrade -y --no-install-recommends -V -qq \
	&& apt-get install -y --no-install-recommends -V $buildDeps $runtimeDeps \
    && rm -r /var/lib/apt/lists/* \
    \
    && pecl install memcached-2.2.0 ssh2 imagick \
    && docker-php-ext-enable memcached ssh2 imagick \
    \
    && pecl install xdebug-2.5.5 \
    \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysqli soap zip pcntl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    \
    && wget -q https://getcomposer.org/composer.phar \
    && mv composer.phar /usr/local/bin/composer \
    && chmod 755 /usr/local/bin/composer \
    && mkdir /opt/composer \
    && cd /opt/composer \
    && composer -n init -s dev \
    && composer -n config prefer-stable true \
    && composer -n require drush/drush:8.* \
    && ln -s /opt/composer/vendor/bin/drush /usr/local/bin/drush \    
    && composer -n require drupal/coder:8.* \
    && ln -s /opt/composer/vendor/bin/phpcs /usr/local/bin/phpcs \
    && composer -n require sebastian/phpcpd:2.* \
    && ln -s /opt/composer/vendor/bin/phpcbf /usr/local/bin/phpcbf \
    && rm -rf ~/.composer \
    \
    && apt-get purge -y --auto-remove $buildDeps \
    && apt-get clean \
    && mkdir /var/run/sshd

COPY php.ini /usr/local/etc/php/php.ini

RUN set -x \
    && mkdir /www \
    && chmod 777 /www \
    && rm -rf /tmp/* /var/tmp/*

EXPOSE 2223

CMD ["/usr/sbin/sshd", "-D", "-p", "2223"]
