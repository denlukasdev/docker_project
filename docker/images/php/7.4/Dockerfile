FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
        libmhash2 \
        libmcrypt-dev \
        bzip2 \
        libbz2-dev \
        libicu-dev \
        libgmp-dev \
        libpcre3-dev \
        libjpeg-dev \
        libpng-dev \
        libreadline6-dev \
        librecode-dev \
        libsqlite3-dev \
        libssl-dev \
        libxml2-dev \
        libcurl4-gnutls-dev \
        libxslt1-dev \
        libldb-dev \
        libldap2-dev \
        libtidy-dev \
        libpq-dev \
        libzip-dev \
        libjpeg62-turbo-dev \
        libonig-dev \
        libwebp-dev \
        git \
        vim \
        libfreetype6-dev \
        nodejs \
        npm \
     && ln -s /usr/lib/x86_64-linux-gnu/libldap.so /usr/lib/libldap.so \
     && ln -s /usr/lib/x86_64-linux-gnu/liblber.so /usr/lib/liblber.so \
     && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
     && docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp \
     && pecl install mcrypt && docker-php-ext-enable mcrypt \
     && docker-php-ext-install -j$(nproc) bz2 bcmath intl soap gd xsl sockets zip pdo_mysql pdo_pgsql ldap tidy \
     && docker-php-ext-enable opcache \
     && npm install --global bower \
     && version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;")

# Setup xdebug last version (3+)
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Setup xdebug version <3
#RUN pecl install xdebug-2.9.8 && docker-php-ext-enable xdebug

RUN apt-get install -y msmtp mailutils

RUN docker-php-ext-install mysqli pdo pdo_mysql

RUN \
curl -sS -O https://files.magerun.net/n98-magerun2-latest.phar &&  \
curl -sS -o n98-magerun2-latest.phar.sha256 https://files.magerun.net/sha256.php?file=n98-magerun2-latest.phar && \
shasum -a 256 -c n98-magerun2-latest.phar.sha256 && \
chmod +x n98-magerun2-latest.phar && \
mv n98-magerun2-latest.phar /usr/local/bin/n98-magerun2-latest.phar

RUN curl -o /tmp/composer-setup.php https://getcomposer.org/installer \
 && curl -o /tmp/composer-setup.sig https://composer.github.io/installer.sig \
 && php -r "if (hash('SHA384', file_get_contents('/tmp/composer-setup.php')) !== trim(file_get_contents('/tmp/composer-setup.sig'))) { unlink('/tmp/composer-setup.php'); echo 'Invalid installer' . PHP_EOL; exit(1); }" \
 && php /tmp/composer-setup.php --no-ansi --version=2.2.3 --install-dir=/usr/local/bin --filename=composer --snapshot \
 && rm -rf /tmp/composer-setup.php \
 && mkdir /var/www/.composer \
 && chown www-data:www-data /var/www/.composer

EXPOSE 9000

USER www-data
USER root

RUN usermod -u 1000 www-data

# Add magento  aliases
RUN echo 'alias m="bin/magento"' >> ~/.bashrc \
    && echo 'alias mdl="rm -rf generated/* && rm -rf var/cache/* && rm -rf var/page_cache/* && rm -rf var/view_preprocessed/* && rm -rf pub/static/* && php bin/magento setup:upgrade && php bin/magento c:f"' >> ~/.bashrc \
    && echo 'alias mcf="rm -rf var/cache/* var/page_cache/* var/view_preprocessed/*  pub/static/frontend/* pub/static/*/*"' >> ~/.bashrc \
    && echo 'alias mrm="rm -rf generated/*"' >> ~/.bashrc

WORKDIR /var/www/html/server
