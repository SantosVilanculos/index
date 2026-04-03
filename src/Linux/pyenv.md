## required

```sh
sudo apt-get install -y \
 build-essential \
 libffi-dev \
 libreadline-dev \
 libssl-dev \
 zlib1g-dev \
 tk-dev
```

## recommended

```sh
sudo apt-get install -y \
 libbz2-dev \
 liblzma-dev \
 libsqlite3-dev
```

##  install

```sh
 curl https://pyenv.run | bash
```

## configuration

### .bashenv/.zshenv

```sh
if [ -d "$HOME/.pyenv/bin" ]; then
  export PATH="$PATH:$HOME/.pyenv/bin"
fi

if [ -n "$(command -v pyenv)" ]; then
  eval "$(pyenv init - zsh)"
  eval "$(pyenv virtualenv-init -)"
fi
```

## python

```sh
pyenv install 3
pyenv global 3
```
