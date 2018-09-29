FROM php:5.6-fpm

RUN set -x \
	&& buildDeps=" \
        autoconf \
        automake \
        cpp \
        g++ \
        gcc \
        git \
        make \
        libfreetype6-dev \
        libmagickwand-dev \
        libjpeg62-turbo-dev \
        libmemcached-dev \
        libpng-dev \
        libssh2-1-dev \
        libssl-dev \
        libtool \
        libxml2-dev \
        perl \
	" \
    && runtimeDeps=" \
        ca-certificates \
        imagemagick \
        libfreetype6 \
        libgeos-c1v5 \
        libjpeg62-turbo \
        libmemcached11 \
        libmemcachedutil2 \
        libpng16-16 \
        libssh2-1 \
        libxml2 \
        openssl \
    " \
	&& apt-get update \
    && apt-get dist-upgrade -y --no-install-recommends -V -qq \
	&& apt-get install -y --no-install-recommends -V $buildDeps $runtimeDeps \
    && rm -r /var/lib/apt/lists/* \
    \
    && pecl channel-update pecl.php.net \
    && pecl install memcached-2.2.0 ssh2 uploadprogress xdebug-2.5.5 imagick \
    && docker-php-ext-enable memcached ssh2 uploadprogress imagick opcache \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysqli soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    \
    && rm /usr/local/etc/php-fpm.d/zz-docker.conf \
    && apt-get purge -y --auto-remove $buildDeps \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

# PHP Configuration
COPY php.ini /usr/local/etc/php/php.ini
COPY www.conf /usr/local/etc/php-fpm.d/www.conf

EXPOSE 9000
