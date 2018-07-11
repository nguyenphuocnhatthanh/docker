FROM ubuntu:16.04
MAINTAINER Nguyen Phuoc Nhat Thanh <nguyenphuocnhatthanh@gmail.com>
USER root
RUN apt-get update && apt-get -y install \
    language-pack-en-base \
    software-properties-common
RUN export LC_ALL=en_US.UTF-8 && export LANG=en_US.UTF-8 && add-apt-repository ppa:ondrej/php && apt-get update
RUN apt-get -y install \
    apache2 \
    php7.1 \
    php7.1-cli \
    php7.1-pgsql \
    php7.1-common \
    php7.1-json \
    php7.1-xml \
    php7.1-gd \
    php7.1-imap \
    php7.1-mbstring \
    php7.1-mysqlnd \
    php7.1-opcache \
    php7.1-pdo \
    php7.1-intl \
    php7.1-mcrypt \
     php7.1-curl \
    curl \
    git \
    unzip \
    vim \
    redis-server \
    supervisor
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get -y install \
    nodejs
RUN a2enmod rewrite
RUN a2enmod headers
RUN a2enmod ssl
RUN npm install -g laravel-echo-server
#RUN laravel-echo-server init

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.29.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
ENV SHIPPABLE_NODE_VERSION=v6.9.5
RUN . $HOME/.nvm/nvm.sh && nvm install $SHIPPABLE_NODE_VERSION && nvm alias default $SHIPPABLE_NODE_VERSION && nvm use default

RUN mkdir /etc/apache2/ssl 2> /dev/null

RUN openssl genrsa -out "/etc/apache2/ssl/gallery.key" 2048 \
    && openssl req -new -key "/etc/apache2/ssl/gallery.key" -out "/etc/apache2/ssl/gallery.csr" -subj "/CN=gallery.local/O=LGS/C=IT" \
    && openssl x509 -req -days 365 -in "/etc/apache2/ssl/gallery.csr" -signkey "/etc/apache2/ssl/gallery.key" -out "/etc/apache2/ssl/gallery.crt"

ADD default-ssl.conf /etc/apache2/sites-enabled/default-ssl.conf

WORKDIR /var/www/html
EXPOSE 80 443 6001
ADD startup.sh /usr/bin/startup.sh
RUN chmod +x /usr/bin/startup.sh

# Execute the containers startup script which will start many processes/services
CMD ["/bin/bash", "/usr/bin/startup.sh"]


