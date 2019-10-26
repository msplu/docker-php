FROM php:7.2-cli-alpine

RUN apk add --update --no-cache \
    libmcrypt-dev \
    libpng-dev \
    libjpeg-turbo-dev \
    freetype-dev && \
	docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install pdo pdo_mysql gd
