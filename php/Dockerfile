FROM ubuntu:trusty

COPY sources.list /etc/apt/sources.list

# supervisord
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
	supervisor \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# apache
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apache2 \
    libapache2-mod-rpaf \
    libapache2-mod-fastcgi \
    software-properties-common \
    python-software-properties \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN a2enmod rewrite && \
    a2enmod rpaf && \
    a2enmod actions && \
    a2enmod fastcgi && \
    a2enmod headers && \
    a2enmod proxy_http && \
    a2disconf other-vhosts-access-log && \
    mkdir -p /var/www/html

RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    git \
    curl \
    php7.1-fpm \
    php7.1-cli \
    php7.1-mbstring \
    php7.1-xml \
    php7.1-soap \
    php7.1-curl \
    php7.1-mcrypt \
    php7.1-gd \
    php7.1-bz2 \
    php7.1-zip \
    php7.1-mysql \
    php7.1-mongo \
    php7.1-memcached \
    php-xdebug \
    imagemagick \
    mysql-client \
    && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


#COPY php.ini /usr/local/etc/php/
#COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
#RUN apt-get update \
#  && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev libmcrypt-dev mcrypt wget git \
#  && docker-php-ext-install pdo_mysql mysqli mbstring gd iconv mcrypt zip

RUN phpdismod opcache && \
    curl -o /usr/local/bin/composer https://getcomposer.org/composer.phar && \
    chmod +x /usr/local/bin/composer && \
    mkdir /opt/drush && \
    cd /opt/drush && \
    composer require drush/drush:^8.1.8 && \
    ln -s /opt/drush/vendor/bin/drush /usr/local/bin/drush && \
    curl https://drupalconsole.com/installer -L -o /usr/local/bin/drupal && \
    chmod +x /usr/local/bin/drupal

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
#COPY files/apache2.conf /etc/apache2/apache2.conf

COPY php.ini /etc/php/7.1/fpm/php.ini
COPY php.ini /etc/php/7.1/cli/php.ini
COPY php-fpm.conf /etc/php/7.1/fpm/php-fpm.conf
COPY php-fpm-pool.conf /etc/php/7.1/fpm/pool.d/www.conf
COPY start.sh /

RUN chmod 755 /start.sh

EXPOSE 80

ENV HOME=/tmp

WORKDIR /var/www/html/web

ENTRYPOINT ["/usr/bin/supervisord"]
