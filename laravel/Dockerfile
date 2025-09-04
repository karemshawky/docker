FROM php:8.3-fpm-alpine

# Copy composer files
COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www

# Install system dependencies
RUN apk add --no-cache \
    curl \
    build-base \
    libpng-dev \
    libxml2-dev \
    libzip-dev \
    oniguruma-dev \
    zip \
    unzip \
    openssl

# Install extensions
RUN docker-php-ext-install pdo pdo_mysql mbstring zip exif pcntl bcmath opcache

# Install composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Add user for Laravel application
RUN addgroup -g 1000 www \
    && adduser -u 1000 -G www -s /bin/sh -D www

# Copy application files and set permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
