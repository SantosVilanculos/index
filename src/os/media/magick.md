## install

### debian

```sh
sudo apt install imagemagick
```

## use

### convert

#### multisize \*.ico

```sh
convert ./favicon.png -define icon:auto-resize="256,128,96,64,48,32,16" ./favicon.ico
```

### resize

```sh
convert ./favicon.png -resize x152 ./favicon.png ./apple-touch-icon-152x152.png
```
