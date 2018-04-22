FROM php:7.2.3-fpm

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} appuser && \
    useradd -u ${PUID} -g appuser -m appuser && \
    usermod -p "*" appuser

ARG TZ=Asia/Tokyo
ENV TZ ${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
    build-essential \
    mysql-client \
    openssl \
    locales \
    zip \
    zlib1g-dev

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install \
    pdo_mysql \
    zip

COPY php.ini /usr/local/etc/php/php.ini

RUN echo ja_JP.UTF-8 UTF-8 > /etc/locale.gen && locale-gen

RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/bin --filename=composer

USER appuser
RUN composer config -g repos.packagist composer https://packagist.jp \
    && composer global require hirak/prestissimo \
    && composer global require laravel/installer

USER root

RUN mkdir -p /var/sessions \
    && chown -R www-data /var/sessions

WORKDIR /var/www/html
