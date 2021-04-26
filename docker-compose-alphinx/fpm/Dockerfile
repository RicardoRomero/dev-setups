FROM php:7.4.16-fpm-alpine3.13

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
# COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync

# Magento PHP dependencies
RUN install-php-extensions bcmath \
    gd \
    intl \
    hash \
    pdo_mysql \
    soap \
    xsl \
    zip \
    sockets

RUN install-php-extensions xdebug
RUN mkdir -p /var/www/html/magento && \
chown -R www-data:www-data /var/www/html

COPY --from=composer:1.10.21 /usr/bin/composer /usr/bin/composer