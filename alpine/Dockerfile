FROM php:7.1-fpm-alpine

RUN apk add --update libmcrypt-dev icu-dev libpng-dev && \
    docker-php-ext-install pdo pdo_mysql gd mcrypt intl && \
    rm -rf /var/cache/apk/*

