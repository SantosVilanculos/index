## install

### LSP
```sh
curl -Lo phpactor.phar https://github.com/phpactor/phpactor/releases/latest/download/phpactor.phar
chmod a+x phpactor.phar
mv phpactor.phar ~/.local/bin/phpactor
```

## VSCode
```sh
curl -Lo phpactor.vsix https://github.com/phpactor/vscode-phpactor/releases/latest/download/phpactor.vsix
code --install-extension phpactor.vsix
rm phpactor.vsix
```
