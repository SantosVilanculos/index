## required

- php>=8.2

## install/update

```sh
TEMP_DIR=$(mktemp -d)
LATEST_VERSION=$(curl -s "https://api.github.com/repos/composer/composer/releases/latest" | grep -Po '"tag_name": "\K[^"]*')
wget "https://github.com/composer/composer/releases/download/${LATEST_VERSION}/composer.phar" -O "${TEMP_DIR}/composer.phar"
sudo chmod +x "${TEMP_DIR}/composer.phar"
sudo mv "${TEMP_DIR}/composer.phar" "/usr/local/bin/composer"
rm -rf "${TEMP_DIR}"
```

## configuration

### .bashenv/.zshenv

```sh
export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

```sh
composer config --global github-oauth.github.com <token>
```
