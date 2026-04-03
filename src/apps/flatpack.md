## debian bookworm

```sh
sudo apt-get install -y flatpak libfuse2

sudo add-apt-repository ppa:flatpak/stable
sudo apt-get update
sudo apt-get install -y flatpak

sudo apt-get install gnome-software-plugin-flatpak

flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## configuration

### .bashenv/.zshenv

```sh
if [ -d "/var/lib/flatpak/exports/bin" ]; then
  export PATH="$PATH:/var/lib/flatpak/exports/bin"
fi
```
