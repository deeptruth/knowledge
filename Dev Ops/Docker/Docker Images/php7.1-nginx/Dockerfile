FROM ubuntu:latest

MAINTAINER Joshua Clifford Reyes <reyesjoshuaclifford@gmail.com>

RUN apt-get update -y

RUN apt-get install -y software-properties-common
RUN apt-get install -y python-software-properties
RUN LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php

RUN apt-get update --fix-missing -y

# Install nginx, php7.1, etc.
RUN apt-get install -y nginx php7.1 php7.1-fpm php7.1-mbstring php7.1-curl php7.1-mcrypt php7.1-tokenizer php7.1-dom zip vim nano php7.1-mysql wkhtmltopdf libxrender1 php7.1-imap git cron openssl php-bcmath

RUN echo "\ndaemon off;" >> /etc/nginx/site-nginx.conf
RUN sed -i -e "s/;\?daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.1/fpm/php-fpm.conf

RUN echo "\ncgi.fix_pathinfo=0" >> /etc/php/7.1/fpm/php.ini
RUN echo "\nupload_max_filesize = 100M" >> /etc/php/7.1/fpm/php.ini
RUN echo "\npost_max_size = 100M" >> /etc/php/7.1/fpm/php.ini

# Nginx config.
RUN rm /etc/nginx/sites-enabled/default
ADD ./site-nginx.conf /etc/nginx/sites-available/
RUN ln -s /etc/nginx/sites-available/site-nginx.conf /etc/nginx/sites-enabled/site

# Change permission in this folder.
RUN chown -R www-data:www-data /var/www \ 
    && find /var/www -type d -exec chmod 755 {} \; \
    && find /var/www -type f -exec chmod 644 {} \;

# Expose ports.
EXPOSE 80

# Replace the php environment to development mode.
CMD cp /usr/lib/php/7.1/php.ini-development /etc/php/7.1/apache2
CMD mv /etc/php/7.1/apache2/php.ini /etc/php/7.1/apache2/php.ini.bak
CMD mv /etc/php/7.1/apache2/php.ini-development /etc/php/7.1/apache2/php.ini

# Define default command.
ENTRYPOINT service nginx start && service php7.1-fpm start && /bin/bash