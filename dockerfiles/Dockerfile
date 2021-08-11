FROM php:7.4-apache

# apache & phpエクステンション
RUN apt-get update &&\
    apt-get install -y libicu-dev \
    libpq-dev &&\
    docker-php-ext-install intl &&\
    docker-php-ext-install pdo_mysql &&\
    docker-php-ext-install bcmath &&\
    pecl install xdebug &&\
    docker-php-ext-enable xdebug &&\
    # mod_rewrite
    a2enmod rewrite

# composerで使うunzipとロケール設定
RUN apt-get install -y zip unzip locales locales-all git &&\
    echo "ja_JP UTF-8" > /etc/locale.gen &&\
    locale-gen &&\
    update-locale LANG=ja_JP.UTF-8

# vscodeデバッグ
RUN { \
        echo 'xdebug.mode = develop,debug'; \
        echo 'xdebug.start_with_request = yes'; \
        echo 'xdebug.discover_client_host = 0'; \
        echo 'xdebug.remote_handler = "dbgp"'; \
        echo 'xdebug.client_port = 9001'; \
        echo 'xdebug.log=/var/log/xdebug.log'; \
        echo 'xdebug.log_level=1'; \
    } >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini 

# Install Node.js (7月最新lts)
RUN apt install -y npm \
  && npm install n -g \
  && n 14.17.3

# Install Composer　(7月最新)
RUN curl -sS https://getcomposer.org/installer | php \
  && mv composer.phar /usr/local/bin/composer \
  && composer self-update 2.1.3