FROM alpine:edge

MAINTAINER Nickel 980484578@qq.com

ENV TIMEZONE            Asia/Shanghai
ENV PHP_MEMORY_LIMIT    500M
ENV MAX_UPLOAD          100M
ENV PHP_MAX_FILE_UPLOAD 50
ENV PHP_MAX_POST        10M

# set repositories
RUN	echo "http://nl.alpinelinux.org/alpine/latest-stable/main" >> /etc/apk/repositories \
	&& echo "http://nl.alpinelinux.org/alpine/edge/community/" >> /etc/apk/repositories \
	&& echo "http://dl-4.alpinelinux.org/alpine/v3.7/community" >> /etc/apk/repositories \
	&& echo "http://nl.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories \
	&& echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories \
    && apk update && apk upgrade \
	&& apk add --update tzdata \
	&& cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
	&& echo "${TIMEZONE}" > /etc/timezone

# install basic lib for development
RUN apk add --update --no-cache --allow-untrusted \
		gnu-libiconv \
		ca-certificates \
		openssh-client \
		libmemcached-libs \
		musl wget curl bash openssl \
		php7 \
		php7-gd \
		php7-imagick \
		php7-tokenizer \
		\
		php7-openssl \
		php7-sqlite3 \
		php7-gmp \
		php7-pcntl \
		php7-common \
		php7-xsl \
		php7-fpm \
		php7-snmp \
		php7-mbstring \
		php7-timezonedb \
		php7-exif \
		php7-opcache \
		php7-posix \
		php7-session \
		php7-gettext \
		php7-json \
		php7-xml \
		php7-xmlrpc \
		php7-xmlwriter \
		php7-xmlreader \
		php7-simplexml \
		php7-iconv \
		php7-sysvshm \
		php7-mongodb \
		php7-memcached \
		php7-redis \
		php7-curl \
		php7-shmop \
		php7-odbc \
		php7-phar \
		php7-imap \
		php7-mysqlnd \
		php7-mysqli \
		php7-pgsql \
		php7-pdo \
		php7-pdo_dblib \
		php7-pdo_mysql \
		php7-pdo_pgsql \
		php7-pdo_odbc \
		php7-pdo_sqlite \
		php7-zip \
		php7-cgi \
		php7-ctype \
		php7-mcrypt \
		php7-bcmath \
		php7-calendar \
		php7-tidy \
		php7-dom \
		php7-sockets \
		php7-soap \
		php7-sysvmsg \
		php7-zlib \
		php7-ssh2 \
		php7-ftp \
		php7-sysvsem \
		php7-bz2 \
		\
	&& rm -rf /var/cache/apk/* \
	&& rm -rf /tmp/* \
	&& ln -s /usr/sbin/php-fpm7 /usr/sbin/php-fpm

# Fix for iconv: https://github.com/docker-library/php/issues/240
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

# install composer
RUN curl -sS https://getcomposer.org/installer | \
    php7 -- --install-dir=/usr/bin --filename=composer

#COPY php.ini /etc/php7/
RUN	sed -i "s|;*daemonize\s*=\s*yes|daemonize = no|g" /etc/php7/php-fpm.conf && \
	sed -i "s|;*date.timezone =.*|date.timezone = ${TIMEZONE}|i" /etc/php7/php.ini && \
	sed -i "s|;*memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|i" /etc/php7/php.ini && \
    sed -i "s|;*upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|i" /etc/php7/php.ini && \
    sed -i "s|;*max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|i" /etc/php7/php.ini && \
    sed -i "s|;*post_max_size =.*|post_max_size = ${PHP_MAX_POST}|i" /etc/php7/php.ini && \
    sed -i "s|;*cgi.fix_pathinfo=.*|cgi.fix_pathinfo= 0|i" /etc/php7/php.ini && \
    \
    # Cleaning up
	apk del tzdata && \
	rm -rf /var/cache/apk/*

# www user
RUN mkdir /www -p
RUN addgroup -g 1000 -S www  && \
    adduser -u 1000 -D -S -h /www -s /sbin/nologin -G www www

RUN echo "nameserver 8.8.8.8" >> /etc/resolv.conf

ENV PHP_VERSION=7.1.15 \
    IGBINARY_VERSION=2.0.4 \
    PHP_MEMCACHED_VERSION=3.0.3 \
    PHPREDIS_VERSION=3.1.4 \
    PHP_AMQP_VERSION=1.9.0 \
    MONGO_PHP_DRIVER_VERSION=1.1.10 \
    XDEBUG_ENABLE=0 \
    XDEBUG_REMOTE_PORT=9000 \
    XDEBUG_HOST=localhost

ENV NEWRELIC_VERSION=7.7.0.203 \
    NEWRELIC_ENABLED=0 \
    NEWRELIC_APP_NAME="PHP Application" \
    NEWRELIC_LICENSE="" \
    NEWRELIC_LOG_FILE="/dev/stdout" \
    NEWRELIC_LOG_LEVEL="info" \
    NEWRELIC_DAEMON_LOG_FILE="/dev/stdout" \
    NEWRELIC_DAEMON_LOG_LEVEL="info"

ENV PHP_MEMORY_LIMIT=256M \
    PHP_PRECISION=-1 \
    PHP_OUTPUT_BUFFERING=4096 \
    PHP_SERIALIZE_PRECISION=-1 \
    PHP_MAX_EXECUTION_TIME=60 \
    PHP_ERROR_REPORTING=E_ALL \
    PHP_DISPLAY_ERRORS=0 \
    PHP_DISPLAY_STARTUP_ERRORS=0 \
    PHP_TRACK_ERRORS=0 \
    PHP_LOG_ERRORS=1 \
    PHP_LOG_ERRORS_MAX_LEN=10240 \
    PHP_POST_MAX_SIZE=20M \
    PHP_MAX_UPLOAD_FILESIZE=10M \
    PHP_MAX_FILE_UPLOADS=20 \
    PHP_MAX_INPUT_TIME=60 \
    PHP_DATE_TIMEZONE=America/Sao_Paulo \
    PHP_VARIABLES_ORDER=EGPCS \
    PHP_REQUEST_ORDER=GP \
    PHP_SESSION_SERIALIZE_HANDLER=php_binary \
    PHP_SESSION_SAVE_HANDLER=files \
    PHP_SESSION_SAVE_PATH=/tmp \
    PHP_SESSION_GC_PROBABILITY=1 \
    PHP_SESSION_GC_DIVISOR=10000 \
    PHP_OPCACHE_ENABLE=1 \
    PHP_OPCACHE_ENABLE_CLI=0 \
    PHP_OPCACHE_MEMORY_CONSUMPTION=128 \
    PHP_OPCACHE_INTERNED_STRINGS_BUFFER=32 \
    PHP_OPCACHE_MAX_ACCELERATED_FILES=10000 \
    PHP_OPCACHE_USE_CWD=1 \
    PHP_OPCACHE_VALIDATE_TIMESTAMPS=1 \
    PHP_OPCACHE_REVALIDATE_FREQ=2 \
    PHP_OPCACHE_ENABLE_FILE_OVERRIDE=0 \
    PHP_ZEND_ASSERTIONS=-1 \
    PHP_IGBINARY_COMPACT_STRINGS=1 \
    PHP_PM=ondemand \
    PHP_PM_MAX_CHILDREN=100 \
    PHP_PM_START_SERVERS=20 \
    PHP_PM_MIN_SPARE_SERVERS=20 \
    PHP_PM_MAX_SPARE_SERVERS=20 \
    PHP_PM_PROCESS_IDLE_TIMEOUT=60s \
    PHP_PM_MAX_REQUESTS=500

WORKDIR /www

#ENTRYPOINT ["/usr/sbin/php"]
