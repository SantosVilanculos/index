# Ubuntu 24.04.4 (Noble Numbat)

## ssh
```sh
sudo apt install openssh-server -y

sudo systemctl enable --now ssh

sudo ufw allow ssh
sudo ufw --force enable
```

## php

```sh
sudo add-apt-repository -y ppa:ondrej/php

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

sudo systemctl enable php8.4-fpm
sudo systemctl start php8.4-fpm
```
## nginx

```sh
sudo add-apt-repository -y ppa:ondrej/nginx

sudo apt-get update

sudo apt-get install -y nginx

sudo systemctl enable nginx
sudo systemctl start nginx
```

## mysql

```sh
sudo apt-get install -y mysql-server
```

```sh
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
EXIT;
```

---
## configuration

```sh
sudo usermod -aG www-data $USER
sudo chown -R $USER:www-data /var/www
sudo chmod -R 775 /var/www
```

```sh
git clone <url> /var/www/example.com

cd /var/www/example.com

composer install
cp ./.env.example ./.env
php artisan key:generate

pnpm install
pnpm run build
```
set APP_URL and DB_PASSWORD

```sh
php artisan migrate --seed
php artisan optimize
```

```sh
sudo chown -R $USER:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

---

```sh
sudo vim /etc/nginx/sites-available/default
```

```sh
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /var/www/example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

```sh
sudo systemctl reload nginx
```


sudo apt-get install supervisor -y
sudo vim /etc/supervisor/conf.d/example.com.conf

```sh
[program:queue]
process_name=%(program_name)s_%(process_num)02d
command=/usr/bin/php /var/www/example.com/artisan queue:work --sleep=3 --tries=3 --max-time=3600
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=www-data
numprocs=4
redirect_stderr=true
stdout_logfile=/var/www/example.com/storage/logs/queue.log
stopwaitsecs=3600

[program:ssr]
process_name=%(program_name)s_%(process_num)02d
command=/usr/bin/php /var/www/example.com/artisan inertia:start-ssr
autostart=true
autorestart=true
stopasgroup=true
killasgroup=true
user=www-data
numprocs=1
redirect_stderr=true
stdout_logfile=/var/www/example.com/storage/logs/ssr.log
stopwaitsecs=3600
```

sudo supervisorctl update
sudo supervisorctl restart

sudo ufw allow 13714
