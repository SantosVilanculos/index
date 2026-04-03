## required

```sh
sudo apt-get install -y zsh
sudo chsh -s $(which zsh)
```

## install

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended
```

### theme

#### spaceship-prompt/spaceship-prompt

1. clone this repo:

   ```zsh
   git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1
   ```

2. symlink `spaceship.zsh-theme` to your oh-my-zsh custom themes directory:

   ```zsh
   ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
   ```

3. set `ZSH_THEME="spaceship"` in your `.zshrc`.

### plugins

#### zsh-users/zsh-syntax-highlighting

1. clone this repository in oh-my-zsh's plugins directory:

   ```zsh
   git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```

2. activate the plugin in `~/.zshrc`:

   ```zsh
   plugins=( [plugins...] zsh-syntax-highlighting)
   ```

#### zsh-users/zsh-completions

1. clone the repository inside your oh-my-zsh repo:

   ```zsh
   git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions
   ```

2. add it to `FPATH` in your `.zshrc` by adding the following line before `source "$ZSH/oh-my-zsh.sh"`:

   ```zsh
   fpath+=${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions/src
   ```

#### zsh-users/zsh-autosuggestions

1. clone this repository into `$ZSH_CUSTOM/plugins` (by default `~/.oh-my-zsh/custom/plugins`)

   ```sh
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   ```

2. add the plugin to the list of plugins for Oh My Zsh to load (inside `~/.zshrc`):

   ```sh
   plugins=(
       # other plugins...
       zsh-autosuggestions
   )
   ```
