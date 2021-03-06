# Use Alpine Linux
FROM alpine:latest

MAINTAINER Modern Tribe

ENV TIMEZONE             UTC
ENV PHP_MEMORY_LIMIT     512M
ENV MAX_UPLOAD           50M
ENV PHP_MAX_FILE_UPLOAD  200
ENV PHP_MAX_POST         100M

RUN echo "@testing http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "@community http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --update tzdata && \
    cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
    echo "${TIMEZONE}" > /etc/timezone && \
    apk add --update \
        bash \
        curl \
        wget \
        grep \
        php5-xdebug@community \
        php5-memcached@testing \
        php5-mcrypt \
        php5-soap \
        php5-openssl \
        php5-gmp \
        php5-pdo_odbc \
        php5-json \
        php5-dom \
        php5-pdo \
        php5-zip \
        php5-zlib \
        php5-mysqli \
        php5-sqlite3 \
        php5-apcu \
        php5-bcmath \
        php5-gd \
        php5-xcache \
        php5-odbc \
        php5-pdo_mysql \
        php5-pdo_sqlite \
        php5-gettext \
        php5-xmlreader \
        php5-xmlrpc \
        php5-bz2 \
        php5-memcache \
        php5-iconv \
        php5-pdo_dblib \
        php5-curl \
        php5-ctype \
        php5-imap \
        php5-phar \
        php5-fpm && \

    # Cleaning up
    mkdir /www && \
    apk del tzdata && \
    rm -rf /var/cache/apk/*

RUN sed -i "s|;*daemonize\s*=\s*yes|daemonize = no|g" /etc/php5/php-fpm.conf && \
    sed -i "s|;*listen\s*=\s*127.0.0.1:9000|listen = 9000|g" /etc/php5/php-fpm.conf && \
    sed -i "s|;*listen\s*=\s*/||g" /etc/php5/php-fpm.conf && \
    sed -i "s|;catch_workers_output = yes|catch_workers_output = yes|i" /etc/php5/php-fpm.conf && \
    sed -i "s|;clear_env = no|clear_env = no|i" /etc/php5/php-fpm.conf && \
    sed -i "s|error_log = /var/log/php-fpm.log|error_log = /dev/stdout|i" /etc/php5/php-fpm.conf && \

    sed -i "s|;*date.timezone =.*|date.timezone = ${TIMEZONE}|i" /etc/php5/php.ini && \
    sed -i "s|;*memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|i" /etc/php5/php.ini && \
    sed -i "s|;*upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|i" /etc/php5/php.ini && \
    sed -i "s|;*max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|i" /etc/php5/php.ini && \
    sed -i "s|;*post_max_size =.*|post_max_size = ${PHP_MAX_POST}|i" /etc/php5/php.ini && \
    sed -i "s|;*cgi.fix_pathinfo=.*|cgi.fix_pathinfo= 0|i" /etc/php5/php.ini && \
    sed -i "s|;*display_errors =.*|display_errors = on|i" /etc/php5/php.ini && \
    sed -i "s|;*display_startup_errors =.*|display_startup_errors = on|i" /etc/php5/php.ini && \
    sed -i "s|;*error_reporting =.*|error_reporting = -1|i" /etc/php5/php.ini && \

    echo "xdebug.remote_enable=1" >> /etc/php5/conf.d/xdebug.ini && \
    echo "xdebug.remote_autostart=0" >> /etc/php5/conf.d/xdebug.ini && \
    echo "xdebug.remote_connect_back=1" >> /etc/php5/conf.d/xdebug.ini && \
    echo "xdebug.remote_port=9000" >> /etc/php5/conf.d/xdebug.ini && \
    echo "xdebug.force_display_errors=1" >> /etc/php5/conf.d/xdebug.ini


# Tideways
RUN wget https://s3-eu-west-1.amazonaws.com/qafoo-profiler/downloads/tideways-php_2.0.8_x86_64.tar.gz && \
    tar xzvf tideways-php_2.0.8_x86_64.tar.gz && cd tideways && ash install.sh && \
    echo ";extension=tideways.so" >> /etc/php5/conf.d/tideways.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# WP CLI
RUN echo "installing WP-CLI" \
    && curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

WORKDIR /srv/www

# Expose ports
EXPOSE 9000

CMD ["/usr/bin/php-fpm"]