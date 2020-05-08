FROM php:7.3-fpm-alpine

RUN set -eux; \
    apk add --no-cache --virtual .build-deps ${PHPIZE_DEPS}; \
    # Install missing requirement packages for Laravel APP
    docker-php-ext-install -j$(nproc) bcmath pdo_mysql opcache; \
    pecl install redis-5.1.1; \
    docker-php-ext-enable redis; \
    # Install composer
    echo "Installing composer."; \
    EXPECTED_CHECKSUM="$(php -r "echo file_get_contents('https://composer.github.io/installer.sig');")"; \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
    ACTUAL_CHECKSUM="$(php -r "echo hash_file('sha384', 'composer-setup.php');")"; \
    if [ "${ACTUAL_CHECKSUM}" == "${EXPECTED_CHECKSUM}" ]; then \
        echo "Installer verification succeeded."; \
    else \
        echo "Installer verification failed."; \
        exit 1; \
    fi; \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer; \
    php -r "unlink('composer-setup.php');"; \
    composer --version; \
    # Delete PHPIZE_DEPS
    apk del --no-network .build-deps;
