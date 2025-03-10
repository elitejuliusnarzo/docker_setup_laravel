#you can change php version based on your project requirement
FROM php:8.2.13-fpm

RUN apt update && apt install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zlib1g-dev \
    libicu-dev

RUN apt clean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd zip intl

RUN sed -i 's|listen = /run/php/php8.2-fpm.sock|listen = 9000|' /usr/local/etc/php-fpm.d/www.conf

#change app_repository_name with your app repository name (ex. krbs_kids_api)
COPY ./app_repository_name /var/www/html/app_repository_name
WORKDIR /var/www/html/app_repository_name

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

CMD ["php-fpm"]
