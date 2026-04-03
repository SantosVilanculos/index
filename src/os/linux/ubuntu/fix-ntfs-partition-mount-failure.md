# NTFS partition mount failure

> Failed to mount "637 GB Volume" Error mounting/dev/sda4 at/media/<user>/D5E754D005672C00D: wrong fs type, badoption, bad superblock on/ dev/sda4, missing codepage or helper program, orother error

```sh
sudo ntfsfix --clear-dirty /dev/<name>
sudo ntfsfix -d /dev/<name>
```
