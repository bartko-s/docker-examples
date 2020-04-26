FROM php:7.4-apache

RUN usermod -u 1000 www-data

RUN apt-get update

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get install -y libicu-dev \
    && docker-php-ext-install -j$(nproc) intl

RUN docker-php-ext-enable opcache

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN docker-php-ext-install -j$(nproc) pdo_mysql

RUN  apt-get install libzip-dev -y \
    && docker-php-ext-install zip

RUN apt-get install -y libxslt1.1 libxslt1-dev \
    && docker-php-ext-install xsl

COPY ./ssl/ /etc/ssl/
RUN a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod ssl

RUN apt-get install unzip

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql

RUN cd ~ \
    && curl -sS https://getcomposer.org/installer -o composer-setup.php \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer

# fix opravneni aby composer mohol vyuzivat cache aj pod inym uzivatelom ako root
RUN chmod a+w /var/www

RUN rmdir /var/www/html  \
    && mkdir /var/www/src \
    && ln -s /var/www/src/public /var/www/html

COPY php.ini /usr/local/etc/php/

WORKDIR /var/www/src/
