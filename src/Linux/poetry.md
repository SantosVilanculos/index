## install

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

## configuration

### oh my zsh

```sh
mkdir $ZSH_CUSTOM/plugins/poetry
poetry completions zsh > $ZSH_CUSTOM/plugins/poetry/_poetry
```

you must then add poetry to your plugins array in ~/.zshrc:

```sh
plugins(poetry)
``
```
