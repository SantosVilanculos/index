## install

### ubuntu

#### php 8.2/8.3/8.4

```sh
sudo add-apt-repository ppa:ondrej/php

sudo apt-get update

sudo apt-get install -y \
    php8.4 \
    php8.4-bcmath \
    php8.4-cli \
    php8.4-common \
    php8.4-curl \
    php8.4-fpm \
    php8.4-gd \
    php8.4-imagick \
    php8.4-intl \
    php8.4-mbstring \
    php8.4-mcrypt \
    php8.4-mysql \
    php8.4-pgsql \
    php8.4-sqlite3\
    php8.4-tidy \
    php8.4-tokenizer \
    php8.4-xml \
    php8.4-zip
```

---

### debian [bookworm](https://packages.sury.org/php/dists/bookworm)/[bullseye](https://packages.sury.org/php/dists/bullseye)

#### php 8.3/8.4

```sh
sudo apt-get install-y software-properties-common lsb-release apt-transport-https ca-certificates

sudo wget "https://packages.sury.org/php/apt.gpg" -O "/etc/apt/trusted.gpg.d/php.gpg"

echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee "/etc/apt/sources.list.d/php.list"

sudo apt update

sudo apt-get install -y \
    php8.4\
    php8.4-bcmath \
    php8.4-cli \
    php8.4-common \
    php8.4-curl \
    php8.4-fpm \
    php8.4-gd \
    php8.4-imagick \
    php8.4-intl \
    php8.4-mbstring \
    php8.4-mcrypt \
    php8.4-mysql \
    php8.4-pgsql \
    php8.4-sqlite3 \
    php8.4-tidy \
    php8.4-tokenizer \
    php8.4-xml \
    php8.4-zip
```
