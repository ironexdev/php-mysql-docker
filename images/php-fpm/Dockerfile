FROM php:8.0.10-fpm-alpine

ARG BACKEND_CONTEXT_PATH
ARG DOCKER_IMAGE_CONTEXT_PATH
ARG GROUP_ID=1337
ARG USER_ID=1337

RUN addgroup -g $GROUP_ID -S dockeruser

RUN adduser -S -G dockeruser -u $USER_ID -s /bin/ash dockeruser

# PHP gd dependencies
RUN apk add zlib-dev
RUN apk add jpeg-dev
RUN apk add libpng-dev
RUN apk add freetype-dev

# PHP intl dependencies
RUN apk add icu-dev

# PHP soap dependencies
RUN apk add libxml2-dev

# PHP xsl dependencies
RUN apk add libxslt-dev

# PHP zip dependencies
RUN apk add libzip-dev

RUN docker-php-ext-install mysqli
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install bcmath
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd
RUN docker-php-ext-install intl
RUN docker-php-ext-install soap
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip
RUN docker-php-ext-install sockets

# Xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-3.0.4 \
    && docker-php-ext-enable xdebug

# PHP Redis
RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \
        && pecl install redis \
        && docker-php-ext-enable redis.so

COPY --chown=$USER_ID:$GROUP_ID $DOCKER_IMAGE_CONTEXT_PATH/php-fpm/php.ini /usr/local/etc/php/php.ini

# Composer
RUN apk add composer

WORKDIR /home/dockeruser/backend

USER dockeruser

EXPOSE 9000
