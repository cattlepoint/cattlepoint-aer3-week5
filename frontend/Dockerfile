# SugarCRM frontend container
FROM php:8.2-apache

# Required OS packages for PHP extensions
RUN apt-get update && apt-get install -y \
    libpng-dev libjpeg-dev libfreetype6-dev libzip-dev \
    libicu-dev libxslt1-dev libgmp-dev libxml2-dev \
    libonig-dev libcurl4-openssl-dev \
    libc-client-dev libkrb5-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install -j$(nproc) \
    bcmath curl gd gmp imap intl soap xml zip opcache \
    && pecl install redis && docker-php-ext-enable redis \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

# Copy SugarCRM source (assumes it lives in ./sugarcrm)
COPY ./sugarcrm /var/www/html

# Ownership and basic permissions
RUN chown -R www-data:www-data /var/www/html \
    && find /var/www/html -type d -exec chmod 755 {} \; \
    && find /var/www/html -type f -exec chmod 644 {} \;

# Custom PHP settings
COPY docker/php.ini /usr/local/etc/php/conf.d/sugar.ini

EXPOSE 80
CMD ["apache2-foreground"]
