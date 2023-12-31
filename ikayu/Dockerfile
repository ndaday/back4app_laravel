# start from the officiall PHP 7.4 image
FROM php:8.1-apache

# Install necessary libraries
RUN apt-get update && apt-get install -y \
    libonig-dev \
    libzip-dev

# Install PHP extensions
RUN docker-php-ext-install \
    mbstring \
    zip

# Copy Laravel application
COPY . /var/www/html

# Set working directory
WORKDIR /var/www

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Run composer install
RUN composer install

# Remove the default nginx index page
RUN rm -rf /var/www/html

# Change ownership of our applications
RUN chown -R www-data:www-data /var/www/html

RUN docker-php-ext-install mbstring

COPY .env.example .env
RUN php artisan key:generate

# Expose port 80
EXPOSE 80

# Adjusting Apache configurations
RUN a2enmod rewrite
COPY apache-config.conf /etc/apache2/sites-available/000-default.conf