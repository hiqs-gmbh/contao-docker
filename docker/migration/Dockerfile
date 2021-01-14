FROM php:7-apache

ENV APACHE_DOCUMENT_ROOT /var/www/html/contao/web
WORKDIR /var/www/html/contao

RUN apt-get update && apt-get install -y git zip libicu-dev libpng-dev
RUN docker-php-ext-install gd intl pdo_mysql
RUN mkdir -p web && curl https://download.contao.org/contao-manager/stable/contao-manager.phar --output web/contao-manager.phar.php

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN chown -R www-data:www-data ./
RUN a2enmod rewrite
