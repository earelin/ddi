FROM php:7.1-fpm

RUN set -x \
	&& buildDeps=" \
        g++ \
        gcc \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libssh2-1-dev \
        libxml2-dev \
	" \
    && runtimeDeps=" \
        apt-transport-https \
        ca-certificates \
        imagemagick \
        gnupg2 \
        libfreetype6 \
        libjpeg62-turbo \
        libpng16-16 \
        libssh2-1 \
        libxml2 \
    " \
	&& apt-get update \
    && apt-get dist-upgrade -y --no-install-recommends -V -qq \
	&& apt-get install -y --no-install-recommends -V $buildDeps $runtimeDeps \
    && rm -r /var/lib/apt/lists/* \
    \
    && pecl install xdebug redis \
    && docker-php-ext-enable opcache redis \
    \
    && git clone https://git.php.net/repository/pecl/php/uploadprogress.git \
    && cd uploadprogress \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && cd .. \
    && rm -rf uploadprogress \
    && docker-php-ext-enable uploadprogress \
    \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysqli soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    \
    && rm /usr/local/etc/php-fpm.d/docker.conf \
    && apt-get purge -y --auto-remove $buildDeps \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

COPY php.ini /usr/local/etc/php/php.ini
COPY www.conf /usr/local/etc/php-fpm.d/www.conf

EXPOSE 9000
