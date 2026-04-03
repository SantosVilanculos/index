## install

```sh
sudo apt-get install -y apache2
```

## integration

### php

```sh
sudo apt-get install -y libapache2-mod-php

echo "DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm" | sudo tee /etc/apache2/mods-enabled/dir.conf

sudo systemctl restart apache2.service
```
