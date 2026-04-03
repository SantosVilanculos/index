## install

```sh
flatpak install -y flathub io.github.zen_browser.zen

sudo ln -s /var/lib/flatpak/exports/bin/app.zen_browser.zen /usr/local/bin/zen

```

## configuration

```sh
xdg-mime default app.zen_browser.zen.desktop x-scheme-handler/http
xdg-mime default app.zen_browser.zen.desktop x-scheme-handler/https
xdg-mime default app.zen_browser.zen.desktop text/html

xdg-settings set default-web-browser app.zen_browser.zen.desktop
```
