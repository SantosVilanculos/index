## install/update

```sh
curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh
```

## configuration

### .bashenv

```sh
if [ -x "$HOME/.local/bin/zoxide" ]; then
  eval "$(zoxide init bash)"

  alias cd="z"
fi
```

### .zshenv

```sh
if [ -x "$HOME/.local/bin/zoxide" ]; then
  eval "$(zoxide init zsh)"

  alias cd="z"
fi
```
