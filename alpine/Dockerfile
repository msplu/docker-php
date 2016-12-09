FROM php:7.1-fpm-alpine

RUN apk add --update libmcrypt-dev icu-dev libpng-dev libjpeg && \
    docker-php-ext-install pdo pdo_mysql gd mcrypt intl opcache

# apcu
RUN docker-php-source extract && \
    apk add --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    apk del .phpize-deps-configure && \
    docker-php-source delete

RUN rm -rf /var/cache/apk/*
