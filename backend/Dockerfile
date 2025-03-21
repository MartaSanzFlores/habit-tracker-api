FROM php:8.3-fpm-alpine

# Ajouter les dépôts communautaires pour obtenir les paquets nécessaires
RUN apk update && apk add --no-cache icu-libs icu-dev nodejs npm

# Installer les extensions PHP
RUN docker-php-ext-install mysqli pdo pdo_mysql intl opcache

# Copier Composer depuis l'image officielle
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Ajouter la configuration personnalisée d'OPcache
ADD opcache.ini $PHP_INI_DIR/conf.d/

# Définir le répertoire de travail
WORKDIR /code

# Copier uniquement composer.json et composer.lock
COPY composer.json composer.lock ./

# Installer les dépendances
RUN composer install --no-dev --no-scripts --prefer-dist && \
    rm -rf /root/.composer/cache/*

# Copier le reste du projet
COPY . .

# Assurer que les dossiers de cache et de stockage ont les bonnes permissions
RUN chmod -R 775 /code/storage /code/bootstrap/cache && \
    chown -R www-data:www-data /code/storage /code/bootstrap/cache

