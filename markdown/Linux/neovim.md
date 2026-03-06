## install

```sh
LATEST_VERSION=$(curl -s "https://api.github.com/repos/neovim/neovim-releases/releases/latest" | grep -Po '"tag_name": "\K[^"]*')
wget "https://github.com/neovim/neovim-releases/releases/download/${LATEST_VERSION}/nvim-linux-x86_64.deb" -O "./nvim-linux64.deb"
sudo dpkg -i "./nvim-linux-x86_64.deb"
```
