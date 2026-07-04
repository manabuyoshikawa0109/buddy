# ============================================================
# Laravel (PHP-FPM) コンテナ
#   - HTTP は受けない。nginx から FastCGI(9000番) で呼ばれる
#   - reverb / queue サービスもこの同じイメージを使い回す
# ============================================================
FROM php:8.4-fpm-bookworm

# --- システムパッケージ（PHP拡張のビルドに必要なライブラリ含む） ---
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    unzip \
    zip \
    libzip-dev \
    libonig-dev \
    libicu-dev \
    && rm -rf /var/lib/apt/lists/*

# --- PHP拡張 ---
#   pdo_mysql : MySQL接続（必須）
#   mbstring  : マルチバイト文字（多言語サービスなので必須）
#   intl      : 国際化（言語・地域処理）
#   bcmath/zip/exif/pcntl : Laravel/キュー/Reverbでよく使う
RUN docker-php-ext-install \
    pdo_mysql \
    mbstring \
    intl \
    bcmath \
    zip \
    exif \
    pcntl

# --- Redis拡張 (phpredis) ---
RUN pecl install redis \
    && docker-php-ext-enable redis

# --- Composer（公式イメージからバイナリだけコピー） ---
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# 開発時はソースを bind mount するので COPY しない。
# 本番ビルドに切り替える場合はここで：
#   COPY . .
#   RUN composer install --no-dev --optimize-autoloader
#   RUN chown -R www-data:www-data storage bootstrap/cache

EXPOSE 9000
CMD ["php-fpm"]
