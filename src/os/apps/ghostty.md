## install

```sh
sudo apt-get install -y libgtk-4-dev libadwaita-1-dev git

wget https://ziglang.org/download/0.13.0/zig-linux-x86_64-0.13.0.tar.xz
tar -xf ./zig-linux-x86_64-0.13.0.tar.xz
sudo mv ./zig-linux-x86_64-0.13.0/lib /usr/local/lib/zig
sudo mv ./zig-linux-x86_64-0.13.0/zig /usr/local/bin/zig
rm -rf ./zig-linux-x86_64-0.13.0

git clone git@github.com:ghostty-org/ghostty.git
cd ghostty
git checkout v1.1.2

sudo zig build -p /usr -Doptimize=ReleaseFast
```

With a typical Freedesktop-compatible desktop environment (i.e. Gnome, KDE), this will make Ghostty available as an app in your app launcher. If you don't see it immediately you may have to log out and log back in or maybe even restart.
