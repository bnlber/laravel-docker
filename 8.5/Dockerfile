FROM php:8.5

# Add docker-php-extension-installer script
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# Nodesource
RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash -

# Install dependencies
RUN apt-get -y --no-install-recommends install \
    curl \
    git \
    nodejs

# Install php extensions
RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions \
    @composer \
    redis-stable \
    imagick \
    bcmath \
    calendar \
    exif \
    gd \
    intl \
    pcov-stable \
    pdo_mysql \
    pdo_pgsql \
    pcntl \
    soap \
    sockets \
    zip

# Add local and global vendor bin to PATH.
ENV PATH ./vendor/bin:/composer/vendor/bin:/root/.composer/vendor/bin:/usr/local/bin:$PATH

# More generous defaults
RUN echo "memory_limit=512M" > /usr/local/etc/php/conf.d/docker-php-memlimit.ini

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"

# Preinstall Playwright (Browsers and Deps)
# Saves time during the pipelines, although npm still runs for other packages.
RUN npm i -g @playwright/test
#RUN npx playwright install --with-deps
#RUN npx playwright install-deps chromium-headless-shell
RUN npx playwright install --with-deps chromium-headless-shell
RUN npm uninstall -g @playwright/test

# Tidyup
RUN apt-get dist-clean

# Setup working directory
WORKDIR /var/www
