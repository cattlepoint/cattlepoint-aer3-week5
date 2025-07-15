# SugarCRM CE 6.5.26 – PHP 7.4 + Apache
FROM php:7.4-apache

# core runtime deps
RUN apt-get update && apt-get install -y \
    libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libicu-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd intl mbstring bcmath curl soap xml zip \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

# pull the public CE archive, unpack, fix perms
ARG SUGAR_VER=6.5.26
RUN curl -L -o /tmp/sugar.zip \
    "https://downloads.sourceforge.net/project/sugarcrm/1%20-%20SugarCRM%206.5.X/SugarCommunityEdition-6.5.X/SugarCE-Full-${SUGAR_VER}.zip" \
    && unzip /tmp/sugar.zip -d /var/www/html \
    && mv /var/www/html/SugarCE-Full-${SUGAR_VER}/* /var/www/html/ \
    && rm -rf /tmp/sugar.zip /var/www/html/SugarCE-Full-${SUGAR_VER} \
    && chown -R www-data:www-data /var/www/html

EXPOSE 80
CMD ["apache2-foreground"]
