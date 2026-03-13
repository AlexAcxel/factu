FROM php:8.2-fpm-alpine

# Installation des extensions PHP nécessaires
RUN apk add --no-cache \
    git \
    unzip \
    autoconf \
    g++ \
    make \
    icu-dev \
    libpq-dev \
    oniguruma-dev \
    && docker-php-ext-install \
        pdo \
        pdo_pgsql \
        pgsql \
        intl \
        opcache \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && apk del autoconf g++ make

# Installation de Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# Copie des fichiers de dépendances en premier pour profiter du cache Docker
COPY composer.json composer.lock symfony.lock ./

# Installation des dépendances (sans les scripts pour éviter les erreurs au build)
RUN composer install --no-dev --no-scripts --no-autoloader --prefer-dist

# Copie du reste des sources
COPY . .

# Génération de l'autoloader optimisé
RUN composer dump-autoload --optimize

# Permissions sur le dossier var/
RUN chown -R www-data:www-data var/

EXPOSE 9000
CMD ["php-fpm"]
