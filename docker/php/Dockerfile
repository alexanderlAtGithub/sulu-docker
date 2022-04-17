FROM php:8.1.4-apache-bullseye as apache_php

# Install packages
RUN apt-get update && apt-get install -y  openssl  git  unzip  libicu-dev  libzip-dev  libmagickwand-dev  inkscape

# Install PHP extensions
RUN docker-php-ext-configure intl

RUN docker-php-ext-install -j$(nproc)  intl  pdo  pdo_mysql  opcache  zip  gd

RUN pecl install imagick && docker-php-ext-enable imagick

# Enable development PHP configuration and copy project configuration
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
COPY php.ini "$PHP_INI_DIR/conf.d/custom.ini"

# Enable apache modules and copy project configuration
RUN /usr/sbin/a2enmod rewrite && /usr/sbin/a2enmod headers && /usr/sbin/a2enmod expires
COPY ./apache.conf "$APACHE_CONFDIR/sites-available/000-default.conf"
COPY ./apache.conf "$APACHE_CONFDIR/sites-enabled/000-default.conf"


FROM apache_php AS xdebug_apache_php

RUN pecl install xdebug && docker-php-ext-enable xdebug; \
  echo "xdebug.mode=develop,debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
  echo "xdebug.discover_client_host=true" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
  echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini; \
  echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


FROM apache_php AS composer_php

RUN cd /tmp
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer
ENV COMPOSER_HOME /tmp/.composer
