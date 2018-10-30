FROM php:7.1-fpm-alpine

RUN apk add --update --no-cache \
    bash \
    sudo \
    openssh \
    libmcrypt-dev \
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
    docker-php-ext-install pdo pdo_mysql gd mcrypt intl opcache zip mysqli

# apcu
RUN docker-php-source extract && \
    apk add --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    pecl install apcu && \
    docker-php-ext-enable apcu && \
    apk del .phpize-deps-configure && \
    docker-php-source delete

# memcached
RUN apk add --no-cache libmemcached-dev zlib-dev cyrus-sasl-dev git && \
    docker-php-source extract && \
    git clone --branch php7 https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached/ && \
    docker-php-ext-configure memcached && \
    docker-php-ext-install memcached && \
    docker-php-source delete && \
    apk del --no-cache zlib-dev cyrus-sasl-dev git

# yaml
RUN pecl channel-update pecl.php.net
RUN pecl install yaml-2.0.0 && docker-php-ext-enable yaml

# xhprof
RUN docker-php-source extract && \
    apk add --no-cache git && \
    apk add --no-cache --virtual .phpize-deps-configure $PHPIZE_DEPS && \
    git clone https://github.com/longxinH/xhprof.git ./xhprof && \
    cd xhprof/extension/ && \
    /usr/local/bin/phpize && \
    ./configure --with-php-config=/usr/local/bin/php-config && \
    make && sudo make install && \
    docker-php-ext-enable xhprof && \
    apk del --no-cache .phpize-deps-configure git && \
    docker-php-source delete

RUN rm -rf /var/cache/apk/*

RUN sed -i "s/^user\s*=.*/user = root/" /usr/local/etc/php-fpm.d/www.conf
RUN sed -i "s/^group\s*=.*/group = root/" /usr/local/etc/php-fpm.d/www.conf

CMD php-fpm -R
