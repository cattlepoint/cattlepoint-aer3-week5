FROM php:7.4-apache

RUN apt-get update && apt-get install -y libpng-dev libjpeg-dev libfreetype6-dev libzip-dev libicu-dev
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd intl mbstring bcmath curl soap xml zip
RUN a2enmod rewrite
RUN rm -rf /var/lib/apt/lists/*
ARG SUGAR_VER=6.5.26
RUN curl -L -o /tmp/sugar.zip \
    "https://downloads.sourceforge.net/project/sugarcrm/1%20-%20SugarCRM%206.5.X/SugarCommunityEdition-6.5.X/SugarCE-Full-${SUGAR_VER}.zip"
RUN unzip /tmp/sugar.zip -d /var/www/html
RUN mv /var/www/html/SugarCE-Full-${SUGAR_VER}/* /var/www/html/
RUN rm -rf /tmp/sugar.zip /var/www/html/SugarCE-Full-${SUGAR_VER}
RUN chown -R www-data:www-data /var/www/html
EXPOSE 80
CMD ["apache2-foreground"]
