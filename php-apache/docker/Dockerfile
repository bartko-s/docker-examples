FROM php:7.2.11-apache

RUN usermod -u 1000 www-data

RUN apt-get update

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \   
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd
    
RUN apt-get install -y libicu-dev \
    && docker-php-ext-install -j$(nproc) intl

RUN docker-php-ext-enable opcache

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug   

RUN docker-php-ext-install -j$(nproc) pdo_mysql

RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql

RUN docker-php-ext-install zip

RUN pecl install mongodb \
    && docker-php-ext-enable mongodb

RUN apt-get install -y libxslt1.1 libxslt1-dev \
    && docker-php-ext-install xsl

RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key \ 
    -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=AT/ST=Vienna/L=Vienna/O=Security/OU=Development/CN=example.com" \
    && a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod ssl

RUN cd ~ \
    && curl -sS https://getcomposer.org/installer -o composer-setup.php \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer

RUN rmdir /var/www/html  \
    && mkdir /var/www/src \
    && ln -s /var/www/src/public /var/www/html

COPY php.ini /usr/local/etc/php/
