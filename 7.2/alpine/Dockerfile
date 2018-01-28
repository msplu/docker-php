FROM php:7.2-fpm-alpine

RUN apk add --update bash sudo openssh libmcrypt-dev icu-dev libpng-dev libjpeg-turbo-dev freetype-dev argon2 && \
    docker-php-ext-configure gd \
        --with-gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install pdo pdo_mysql gd intl opcache

# apcu & mcrypt
RUN docker-php-source extract && \
    apk add --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    pecl install apcu && \
    pecl install -f mcrypt && \
    docker-php-ext-enable apcu mcrypt && \
    apk del .phpize-deps-configure && \
    docker-php-source delete

RUN rm -rf /var/cache/apk/*

RUN sed -i "s/^user\s*=.*/user = root/" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/^group\s*=.*/group = root/" /usr/local/etc/php-fpm.d/www.conf

CMD php-fpm -R
