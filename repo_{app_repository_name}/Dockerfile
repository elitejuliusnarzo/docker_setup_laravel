FROM php:8.2.13-fpm

ARG USER
ARG APP_REPOSITORY_NAME

# Set default values if not provided
ENV USER=${USER:-elite}
ENV APP_REPOSITORY_NAME=${APP_REPOSITORY_NAME:-elite}

# Create group and user
RUN groupadd --gid 1000 ${USER} && \
    useradd --uid 1000 --gid 1000 --shell /bin/bash --home /home/${USER} ${USER}

RUN mkdir -p /home/${USER} && chown ${USER}:${USER} /home/${USER}

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

RUN sed -i 's|listen = .*|listen = 9000|' /usr/local/etc/php-fpm.d/www.conf

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copy application code
COPY ./${APP_REPOSITORY_NAME} /var/www/html/${APP_REPOSITORY_NAME}

WORKDIR /var/www/html/${APP_REPOSITORY_NAME}

USER ${USER}

CMD ["php-fpm"]
