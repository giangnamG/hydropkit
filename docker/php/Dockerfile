FROM php:fpm-alpine

# Cài đặt các tiện ích cần thiết và phần mở rộng PHP
RUN apk --no-cache add curl unzip git libpng-dev libzip-dev libjpeg-turbo-dev freetype-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql mysqli bcmath gd zip \
    && docker-php-ext-enable pdo_mysql mysqli bcmath gd zip


# Cài đặt Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Đặt thư mục làm việc
WORKDIR /var/www/html

# Sao chép mã nguồn của dự án vào container
COPY --chown=www-data:www-data ./cms /var/www/html/

# Sửa quyền user và group cho www-data
RUN deluser www-data \
    && addgroup -g 1001 www-data \
    && adduser -u 1000 -G www-data -s /bin/sh -D www-data

# Thiết lập quyền trên thư mục /var/www/html
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

# Chạy composer install để cài đặt các package PHP (nếu có composer.json)
RUN if [ -f /var/www/html/composer.json ]; then composer install; fi


# Hiển thị port 9000 (cổng PHP-FPM)
EXPOSE 9000
