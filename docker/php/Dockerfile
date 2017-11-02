FROM php:5.6-fpm

# Some stuff
RUN apt-get update && apt-get install -y \
        vim-tiny \
        curl \
        wget \
        unzip \
        ssh

# Install git 2.7.4
RUN apt-get update && apt-get install -y \
        build-essential \
        zlib1g \
        zlib1g-dev \
        libcurl4-gnutls-dev \
        libexpat1-dev \
        gettext \
        libz-dev \
        libssl-dev

RUN mkdir /tmp/git && cd /tmp/git && \
        wget https://www.kernel.org/pub/software/scm/git/git-2.7.4.tar.gz && \
        tar -zxf git-2.7.4.tar.gz && \
        cd git-2.7.4 && \
        make prefix=/usr/local all && \
        make prefix=/usr/local install


# Memcached
RUN apt-get update && apt-get install -y libz-dev libmemcached-dev
RUN pecl install memcached-2.2.0
RUN echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini

# APC
RUN pecl install apcu-4.0.11
RUN echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini

# XDebug
RUN yes | pecl install xdebug \
    && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_host=172.20.1.1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.profiler_enable_trigger = 1" >> /usr/local/etc/php/conf.d/xdebug.ini

# Copy php.ini
ADD ./php.ini /usr/local/etc/php/

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG GID
RUN usermod -u ${GID} www-data


WORKDIR "/var/www"

EXPOSE 9000
