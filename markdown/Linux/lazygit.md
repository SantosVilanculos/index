## install

```sh
TEMP_DIR=$(mktemp -d)
LATEST_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*')
wget "https://github.com/jesseduffield/lazygit/releases/download/v${LATEST_VERSION}/lazygit_${LATEST_VERSION}_linux_x86_64.tar.gz" -O "${TEMP_DIR}/lazygit_${LATEST_VERSION}_linux_x86_64.tar.gz"
tar -xf "${TEMP_DIR}/lazygit_${LATEST_VERSION}_linux_x86_64.tar.gz" -C "${TEMP_DIR}"
sudo mv "${TEMP_DIR}/lazygit" /usr/local/bin
sudo chmod +x /usr/local/bin/lazygit
rm -rf "${TEMP_DIR}"
```
