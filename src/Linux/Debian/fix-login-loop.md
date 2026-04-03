## Login loop

1. Option 1

```sh
sudo dpkg-reconfigure gdm3
```

---

2. Option 2

```sh
sudo apt-get purge gdm3
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get clean
sudo apt-get install gdm3
```
