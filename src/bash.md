## command

### command exists

```sh
if [ -n "$(command -v zenity)" ]; then
fi
```

### command does not exist

```sh
if [ -z "$(command -v zenity)" ]; then
fi
```
