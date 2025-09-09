# Pterodactyl Panel Development Environment
# Base: Debian Bookworm
# Maintainer: biraru@biraru.org
# Source: https://github.com/BiraruStudios/development
FROM debian:bookworm-slim

# Metadata
LABEL maintainer="biraru@biraru.org" \
      description="Local development environment for Pterodactyl Panel." \
      org.opencontainers.image.source="https://github.com/BiraruStudios/development"

# Environment
ENV DEBIAN_FRONTEND=noninteractive \
        LANG=C.UTF-8

USER root

# Install system dependencies
RUN apt update -y && apt install -y \
        lsb-release iproute2 iputils-ping locales sudo \
        software-properties-common gnupg2 apt-transport-https \
        ca-certificates curl tar unzip git nano \
        nginx supervisor cron \
    && rm -rf /var/lib/apt/lists/*

# Add external repositories (Yarn and PHP 8.3)
RUN curl -fsSL https://dl.yarnpkg.com/debian/pubkey.gpg \
        | gpg --dearmor -o /usr/share/keyrings/yarn-archive-keyring.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/yarn-archive-keyring.gpg] \
        https://dl.yarnpkg.com/debian stable main" \
        > /etc/apt/sources.list.d/yarn.list \
    \
    && curl -fsSL https://packages.sury.org/php/apt.gpg \
        | gpg --dearmor -o /usr/share/keyrings/sury-php.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/sury-php.gpg] \
        https://packages.sury.org/php/ $(lsb_release -sc) main" \
        > /etc/apt/sources.list.d/sury-php.list

# Install NVM and NodeJS
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" \
    && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && nvm install 14 \
    && nvm use 14 \
    && nvm alias default 14

# Install Yarn, PHP 8.3 and Composer
RUN apt update -y && apt upgrade -y && apt install -y \
      yarn \
      php8.3 php8.3-cli php8.3-common php8.3-gd \
      php8.3-mysql php8.3-mbstring php8.3-bcmath \
      php8.3-xml php8.3-fpm php8.3-curl php8.3-zip \
      php8.3-xdebug php-pear php-dev \
    && curl -sS https://getcomposer.org/installer \
        | php -- --install-dir=/usr/local/bin --filename=composer \
    && apt autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && update-alternatives --install /usr/bin/php php /usr/bin/php8.3 83 \
    && update-alternatives --set php /usr/bin/php8.3

# Copy scripts and configs
COPY entrypoint               /usr/local/bin/entrypoint
COPY setup-pterodactyl        /usr/local/bin/setup-pterodactyl
COPY configs/supervisord.conf /etc/supervisord.conf
COPY configs/pterodactyl.conf /etc/nginx/sites-available/pterodactyl.conf

# User setup, permissions, and Nginx config
RUN useradd -m pterodactyl \
    && usermod -a -g www-data -G sudo pterodactyl \
    && echo "%sudo ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers.d/sudoers \
    && chmod +x /usr/local/bin/setup-pterodactyl \
    && chmod +x /usr/local/bin/entrypoint \
    && rm -rf /etc/nginx/sites-enabled/* \
    && ln -s /etc/nginx/sites-available/pterodactyl.conf /etc/nginx/sites-enabled/ \
    && phpdismod -s cli xdebug \
    && mkdir -p /var/www/html \
    && (crontab -l 2>/dev/null; \
        echo "* * * * * php /var/www/html/artisan schedule:run >/dev/null 2>&1") \
        | crontab -

# Set working directory for Panel
WORKDIR /var/www/html

# Expose ports used by Panel
EXPOSE 80 8080

ENTRYPOINT ["entrypoint"]
