## install

### debian bookworm

```sh
sudo apt-get install -y libfuse2
```

### Ubuntu (>= 24.04)

```sh
sudo add-apt-repository universe
sudo apt install libfuse2t64
```

### Ubuntu (>= 22.04)

```sh
sudo add-apt-repository universe
sudo apt install libfuse2
```

### Ubuntu (<= 21.10)

```sh
sudo apt install fuse libfuse2
sudo modprobe fuse
sudo groupadd fuse

user="$(whoami)"
sudo usermod -a -G fuse $user
```

[https://github.com/AppImage/AppImageKit/wiki/FUSE](https://github.com/AppImage/AppImageKit/wiki/FUSE)
