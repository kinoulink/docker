FROM phusion/baseimage:0.9.19

# Set the env variable DEBIAN_FRONTEND to noninteractive
ARG DEBIAN_FRONTEND=noninteractive
ARG LC_ALL=C.UTF-8

RUN apt-get update && \
	add-apt-repository -y ppa:ondrej/php && \
	apt-get update && \
	apt-get install -y --no-install-recommends  \
			curl unzip \
			php7.1 \
			php7.1-cli \
			php7.1-common \
			php7.1-curl \
			php7.1-dev \
			php7.1-fpm \
			php7.1-gd \
			php7.1-gmp \
			php7.1-intl \
			php7.1-json \
			php7.1-mbstring \
			php7.1-mysql \
			php7.1-oauth \
			php7.1-opcache \
			php-7.1-pdo \
			php7.1-xml \
			php7.1-zip \
			php7.1-yaml \
			nginx-light \
			ffmpeg

RUN mkdir /run/php /etc/service/nginx /etc/service/php /opt/tmp && \
	chown www-data /opt/tmp

ADD services/nginx      /etc/service/nginx/run
ADD services/php        /etc/service/php/run
ADD services/cron       /etc/cron.d/ktv

ADD misc/bashrc     /root/bashrc
ADD misc/init       /etc/my_init.d/00_my_init

RUN echo "source /root/bashrc" >> ~/.bashrc && \
	chmod 600 /etc/cron.d/ktv

RUN mkdir -p /opt/ktv/fs/cache /var/lib/php/session && \
	chown www-data:www-data -R /opt/ktv/fs /var/lib/php/session

ADD config/nginx.conf /etc/nginx/nginx.conf
ADD config/php.ini    /etc/php/7.0/fpm/php.ini
ADD config/php.ini    /etc/php/7.0/cli/php.ini
ADD config/pool.conf  /etc/php/7.0/fpm/pool.d/www.conf

RUN rm -rf /etc/service/{sshd,syslog-forwarder,syslog-ng} && \
    rm -rf /etc/my_init.d/00_regen_ssh_host_keys.sh

EXPOSE 80

RUN rm -rf /var/lib/apt/lists/*

ENV STAGE_ENV=prod

WORKDIR /var/www
