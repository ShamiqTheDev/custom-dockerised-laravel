FROM php:8.3-fpm

# Install required packages and PHP extensions
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libzip-dev \
    unzip \
    && docker-php-ext-install zip pdo_mysql \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create a non-root user and group
RUN groupadd -g 1000 www && useradd -u 1000 -ms /bin/bash -g www www

# Set working directory
WORKDIR /var/www/html

# Copy the application code
COPY . .

# Set permissions and switch to the non-root user
RUN chown -R www:www /var/www/html

# Switch to non-root user
USER www

# Copy composer.json and composer.lock and install dependencies
COPY --chown=www:www composer.json composer.lock ./
RUN composer install --no-dev --optimize-autoloader

# Expose port 9000
EXPOSE 9000

# Start the PHP-FPM server
CMD ["php-fpm"]
