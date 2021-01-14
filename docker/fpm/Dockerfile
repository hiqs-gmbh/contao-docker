FROM php:7-fpm-alpine

ENV TRUSTED_PROXIES 0.0.0.0/0

RUN set -ex \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS imagemagick-dev libtool \
    && export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && pecl install imagick-3.4.3 \
    && docker-php-ext-enable imagick \
    && apk add --no-cache --virtual .imagick-runtime-deps imagemagick \
    && apk del .phpize-deps

RUN apk --no-cache add git zip shadow zlib-dev libpng-dev icu-dev

RUN docker-php-ext-install gd intl pdo_mysql

RUN usermod -u 1000 www-data
RUN apk del shadow
