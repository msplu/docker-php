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
    yaml-dev && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install pdo pdo_mysql gd intl opcache zip mysqli

# apcu
RUN docker-php-source extract && \
    apk add --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    apk del .phpize-deps-configure && \
    docker-php-source delete

# yaml
RUN pecl channel-update pecl.php.net
RUN pecl install yaml && docker-php-ext-enable yaml

RUN rm -rf /var/cache/apk/*

RUN sed -i "s/^user\s*=.*/user = root/" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/^group\s*=.*/group = root/" /usr/local/etc/php-fpm.d/www.conf

CMD php-fpm -R
