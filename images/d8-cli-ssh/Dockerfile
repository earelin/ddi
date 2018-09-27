FROM php:7.1-cli

RUN set -x \
	&& buildDeps=" \
        automake \
        g++ \
        gcc \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libltdl-dev \
        libpng-dev \
        libssh2-1-dev \
        libtool \
        libxml2-dev \
        libyaml-dev \
	" \
    && runtimeDeps=" \
        apt-transport-https \
        ca-certificates \
        ctags \
        git \
        gnupg2 \
        dirmngr \
        imagemagick \
        libfreetype6 \
        libjpeg62-turbo \
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
    && pecl install xdebug redis \
    && docker-php-ext-enable redis \
    \
    && docker-php-ext-install -j$(nproc) pdo_mysql mysqli zip \    
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    \
    && apt-get purge -y --auto-remove $buildDeps \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/* ~/.cache \
    && mkdir /var/run/sshd

# PHP Configuration
COPY php.ini /usr/local/etc/php/php.ini

# PHP libraries
ENV PATH /opt/composer/vendor/bin:$PATH

RUN set -x \
    && wget -q https://getcomposer.org/composer.phar \
    && mv composer.phar /usr/local/bin/composer \
    && chmod 755 /usr/local/bin/composer \
    && mkdir /opt/composer \
    && cd /opt/composer \
    && composer init -s dev \
    && composer config prefer-stable true \
    && composer require --no-progress drush/drush:^9.0.0 \
    && ln -s /opt/composer/vendor/bin/drush /usr/local/bin/drush \
    && rm -rf ~/.composer ~/.config/composer ~/.cache/composer \
    && rm -rf /tmp/* /var/tmp/* ~/.cache

RUN set -x \
    && mkdir /www \
    && chmod 777 /www \
    && rm -rf /tmp/* /var/tmp/*

EXPOSE 2222

CMD ["/usr/sbin/sshd", "-D", "-p", "2222"]
