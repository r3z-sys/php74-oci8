FROM php:7.4-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    unzip \
    wget \
    git \
    libaio1 \
    libnsl-dev \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libxml2-dev \
    libonig-dev \
    libcurl4-openssl-dev \
    gcc \
    make \
    g++ \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install \
        gd \
        mysqli \
        pdo \
        pdo_mysql \
        opcache \
        intl \
        zip \
        bcmath \
        mbstring \
        exif \
        dom \
        curl

# Copy file Instant Client dari lokal ke container
COPY instantclient-basic-linux.x64-11.2.0.4.0.zip /tmp/instantclient-basic.zip
COPY instantclient-sdk-linux-x86-64-11.2.0.2.0.zip /tmp/instantclient-sdk.zip

# Install Oracle Instant Client 11.2 (dari repo eksternal)
RUN apt-get update && apt-get install -y unzip libaio1 \
    && unzip /tmp/instantclient-basic.zip -d /usr/local/ \
    && unzip /tmp/instantclient-sdk.zip -d /usr/local/ \
    && ln -s /usr/local/instantclient_11_2 /usr/local/instantclient \
    && ln -s /usr/local/instantclient/libclntsh.so.* /usr/local/instantclient/libclntsh.so \
    && ln -s /usr/local/instantclient/libocci.so.* /usr/local/instantclient/libocci.so \
    && echo 'instantclient,/usr/local/instantclient' | pecl install oci8-2.2.0 \
    && echo 'extension=oci8.so' > /usr/local/etc/php/conf.d/oci8.ini \
    && echo "/usr/local/instantclient" > /etc/ld.so.conf.d/oracle-instantclient.conf \
    && ldconfig \
    && rm -rf /var/lib/apt/lists/* /tmp/*.zip

# Install Redis extension via PECL
RUN pecl install redis-5.3.7 \
    && docker-php-ext-enable redis

# Set env agar library Oracle dikenali
ENV LD_LIBRARY_PATH="/usr/local/instantclient:${LD_LIBRARY_PATH}"

# Cleanup
RUN apt-get remove -y wget unzip \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www/html

EXPOSE 9000
CMD ["php-fpm"]