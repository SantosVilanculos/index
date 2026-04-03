## required
```sh
sudo apt-get install curl network-manager libnss3-tools jq xsel
```

## install
```sh
composer global require cpriego/valet-linux

valet install

sudo systemctl restart NetworkManager.service
```
