## install

```sh
sudo apt-get install mysql-server
```

## configuration

### password

```sh
sudo mysql
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
EXIT;
```
