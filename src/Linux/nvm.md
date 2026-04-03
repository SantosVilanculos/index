## install

```sh
LATEST_VERSION=$(curl -s "https://api.github.com/repos/nvm-sh/nvm/releases/latest" | grep -Po '"tag_name": "\K[^"]*')
wget -qO- "https://raw.githubusercontent.com/nvm-sh/nvm/${LATEST_VERSION}/install.sh" | bash
```

## configuration

### .bashenv/.zshenv

```sh
if [ -s "$HOME/.nvm/nvm.sh" ]; then
  source "$HOME/.nvm/nvm.sh"
fi

if [ -s "$HOME/.nvm/bash_completion" ]; then
  source "$HOME/.nvm/bash_completion"
fi
```

## node

```sh
nvm install --lts
nvm use --lts
```
