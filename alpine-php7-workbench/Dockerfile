FROM alpine-php7-development

# compile other php ext
ENV PHPIZE_DEPS build-base linux-headers autoconf file g++ gcc libc-dev make pkgconf git automake libtool re2c php7-dev php7-pear yaml-dev libjpeg gmp-dev libaio re2c bison libmcrypt-dev freetype-dev libxpm-dev libwebp-dev libjpeg-turbo-dev bzip2-dev openssl-dev krb5-dev libxml2-dev libaio-dev curl-dev flex-dev boost-dev libevent-dev zlib-dev

RUN set -xe \
    && apk add --no-cache --virtual .phpize-deps \ 
    $PHPIZE_DEPS \
    && sed -i 's/^exec $PHP -C -n/exec $PHP -C/g' $(which pecl) \
    && pecl channel-update pecl.php.net

# kafka
RUN apk add --no-cache librdkafka php7-rdkafka

# protocolbuffers
RUN cd /tmp \
	&& git clone https://github.com/serggp/php-protobuf.git \
	&& cd php-protobuf \
	&& git checkout php7 \
	&& phpize \
	&& ./configure --with-php-config=/usr/bin/php-config \
	&& make \
	&& make install \
    && echo "extension=protobuf.so" > /etc/php7/conf.d/protobuf.ini 

RUN cd /tmp \ 
	&& wget https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.gz \
	&& tar -zxvf protobuf-2.6.1.tar.gz \
	&& cd protobuf-2.6.1 \
	&& ./autogen.sh \
	&& ./configure \
	&& make \
	&& make check \
	&& make install

# ssdb
RUN cd /tmp \
	&& git clone https://github.com/jonnywang/phpssdb.git \
	&& cd phpssdb \
	&& git checkout php7 \
	&& phpize \
	&& ./configure --with-php-config=/usr/bin/php-config \
	&& make \
	&& make install \
	&& echo "extension=ssdb.so" > /etc/php7/conf.d/ssdb.ini

# hprose
RUN pecl install hprose \
    && echo "extension=hprose.so" > /etc/php7/conf.d/hprose.ini 

# yaf
RUN pecl install yaf \
    && echo "extension=yaf.so" > /etc/php7/conf.d/yaf.ini 

# swoole
RUN pecl install swoole \
    && echo "extension=swoole.so" > /etc/php7/conf.d/swoole.ini 

# yaconf
RUN pecl install yaconf \
    && echo "extension=yaconf.so" > /etc/php7/conf.d/yaconf.ini 

# yac
RUN cd /tmp \
	&& wget http://pecl.php.net/get/yac-2.0.2.tgz \
	&& tar -xvf yac-2.0.2.tgz \
    && cd yac-2.0.2 \
	&& phpize \
	&& ./configure --with-php-config=/usr/bin/php-config \
	&& make \
	&& make install \
	&& echo "extension=yac.so" > /etc/php7/conf.d/yac.ini

# thrift
RUN apk add flex boost libevent
RUN cd /tmp \
	&& git clone https://git-wip-us.apache.org/repos/asf/thrift.git \
	&& cd thrift \
	&& ./bootstrap.sh \
	&& ./configure --with-lua=no \
	&& make \
	&& make install

RUN cd /tmp/thrift/lib/php/src/ext/thrift_protocol \
	&& phpize \
	&& ./configure --with-php-config=/usr/bin/php-config \
	&& make \
	&& make install \
	&& echo "extension=thrift_protocol.so" > /etc/php7/conf.d/thrift_protocol.ini

# inotify
RUN pecl install inotify \
    && echo "extension=inotify.so" > /etc/php7/conf.d/inotify.ini

# xdebug: Xdebug MUST be loaded as a Zend extension. And 在php.ini 中还要设置 output_buffering = Off
RUN pecl install xdebug \
    && echo "zend_extension=xdebug.so" > /etc/php7/conf.d/xdebug.ini 


ENV IGBINARY_VERSION=2.0.4 \
    PHP_AMQP_VERSION=1.9.0 \
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

# newrelic
RUN cd /tmp && \
    wget http://download.newrelic.com/php_agent/release/newrelic-php5-${NEWRELIC_VERSION}-linux-musl.tar.gz && \
    tar -xvf newrelic-php5-${NEWRELIC_VERSION}-linux-musl.tar.gz && \
    ./newrelic-php5-${NEWRELIC_VERSION}-linux-musl/newrelic-install install

# not clean for rebuild with cache
#RUN rm -rf /tmp/* && apk del .phpize-deps