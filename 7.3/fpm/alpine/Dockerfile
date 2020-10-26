FROM php:7.3-fpm-alpine

RUN apk add --update --no-cache \
    bash \
    sudo \
    openssh \
    libzip-dev \
    icu-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev \
    g++ \
    make \
    autoconf \
    yaml-dev \
    libxml2-dev \
    bzip2-dev && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install pdo pdo_mysql gd intl opcache zip mysqli bz2 soap

# apcu
RUN docker-php-source extract && \
    apk add --update --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    apk del .phpize-deps-configure && \
    docker-php-source delete

# Memcached
ENV MEMCACHED_DEPS zlib-dev libmemcached-dev cyrus-sasl-dev
RUN apk add --no-cache --update libmemcached-libs zlib
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --update --virtual .memcached-deps $MEMCACHED_DEPS \
    && pecl install memcached \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20_memcached.ini \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .memcached-deps .phpize-deps

# yaml
RUN pecl channel-update pecl.php.net
RUN pecl install yaml && docker-php-ext-enable yaml

RUN rm -rf /var/cache/apk/*

RUN sed -i "s/^user\s*=.*/user = root/" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/^group\s*=.*/group = root/" /usr/local/etc/php-fpm.d/www.conf

CMD php-fpm -R
