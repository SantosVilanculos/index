## required

- ubuntu >= 20.04
- wget

```sh
sudo apt-get install wget
```

- [php](./php.md)
- [apache2](./apache2.md)
- [mariadb](./mariadb.md)

## configuration

### database

create an user and database named `ojs` with password `password`:

```sh
CREATE DATABASE ojs;
GRANT ALL PRIVILEGES ON ojs.* TO 'ojs'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
exit
```

### source code

```sh
cd ~/
wget https://pkp.sfu.ca/ojs/download/ojs-3.4.0-5.tar.gz

tar -xzvf ojs-3.4.0-5.tar.gz

sudo mkdir -p /var/www/ojs/
sudo mv ojs-3.4.0-5/ /var/www/ojs/application   
sudo mkdir -p /var/www/ojs/storage

sudo chown -R www-data:www-data /var/www/ojs/application
sudo chown -R www-data:www-data /var/www/ojs/storage

sudo chmod -R 755 /var/www/ojs/application
sudo chmod -R 755 /var/www/ojs/storage
```

### virtual host

```sh
cat <<'EOF' | sudo /etc/apache2/sites-available/ojs.conf
<VirtualHost *:80>
 DocumentRoot /var/www/ojs/application

 <Directory /var/www/ojs/application>
  Options -Indexes +FollowSymLinks +MultiViews
  AllowOverride All
  Require all granted
 </Directory>

 ErrorLog ${APACHE_LOG_DIR}/ojs.id_error.log
 CustomLog ${APACHE_LOG_DIR}/ojs.id_requests.log combined
</VirtualHost>
EOF

sudo a2dissite 000-default.conf

sudo a2ensite ojs.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## expose

```sh
sudo ufw allow 80  # http (Apache)
sudo ufw allow 443 # https (Apache)
sudo ufw allow 22  # ssh
 ```
