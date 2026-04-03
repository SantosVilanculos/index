## install

### debian

```sh
sudo curl --output-dir /etc/apt/trusted.gpg.d -O https://apt.fruit.je/fruit.gpg

echo "deb http://apt.fruit.je/debian $(lsb_release -cs) mpv" | sudo tee /etc/apt/sources.list.d/fruit.list

sudo apt-get update

sudo apt-get install -y mpv
```
