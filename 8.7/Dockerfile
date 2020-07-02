FROM php:7.2-apache

# copy own php.ini
# COPY config/php.ini /usr/local/etc/php

# Install utilities used by TYPO3 CMS / Flow / Neos RUN apt-get update \
RUN apt-get update \
        && apt-get install -y \
                graphicsmagick \
                zip \
                unzip \
                git \
                libxml2-dev\
                libpng-dev \
        libpq-dev \
        && rm -rf /var/lib/apt/lists/*

# gd
RUN buildRequirements="libpng-dev libjpeg-dev libfreetype6-dev" \
        && apt-get update && apt-get install -y ${buildRequirements} \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/inlcude/ \
        && docker-php-ext-install -j$(nproc) mysqli soap gd zip \
        && apt-get purge -y ${buildRequirements} \
        && rm -rf /var/lib/apt/lists/*

# soap
# used libxml2-dev
RUN docker-php-ext-install soap

# zip
RUN docker-php-ext-install zip

# Install PDO MySQL driver
# See https://github.com/docker-library/php/issues/62
# RUN docker-php-ext-install pdo mysql
RUN docker-php-ext-install pdo mysqli

# Workaround for write permission on write to MacOS X volumes
# See https://github.com/boot2docker/boot2docker/pull/534
RUN usermod -u 1000 www-data

# Enable Apache mod_rewrite
RUN a2enmod rewrite
