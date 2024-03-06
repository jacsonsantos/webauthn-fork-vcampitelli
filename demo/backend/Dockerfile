FROM php:8.2-apache

WORKDIR /var/www

RUN apt update && apt upgrade -y

#install some base extensions
RUN apt install -y \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY ["composer.json", "./"]
RUN composer install --no-dev

COPY . .

## Apache2
COPY extra/apache2-conf/00-webauthn.conf /etc/apache2/sites-available/
RUN a2enmod rewrite \
    && a2enmod headers \
    && a2ensite 00-webauthn \
    && a2dissite 000-default