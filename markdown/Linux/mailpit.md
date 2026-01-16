## install

```sh
TEMP_DIR=$(mktemp -d)
LATEST_VERSION=$(curl -s "https://api.github.com/repos/axllent/mailpit/releases/latest" | grep -Po '"tag_name": "\K[^"]*')
wget "https://github.com/axllent/mailpit/releases/download/${LATEST_VERSION}/mailpit-linux-amd64.tar.gz" -O "${TEMP_DIR}/mailpit-linux-amd64.tar.gz"
tar -xf "${TEMP_DIR}/mailpit-linux-amd64.tar.gz" -C "${TEMP_DIR}"
sudo mv "${TEMP_DIR}/mailpit" /usr/local/bin
sudo chmod +x /usr/local/bin/mailpit
rm -rf "${TEMP_DIR}"
```

```sh
cat <<'EOF' | sudo tee /etc/systemd/system/mailpit.service
[Unit]
Description=An email testing tool capturing emails from your application during development
After=network.target

[Service]
Type=simple
User=www-data
Group=www-data
ExecStart=/usr/local/bin/mailpit
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

## endpoint

| protocol |host   | port  |
| -------- |-------|-------|
| http     |`0.0.0.0`| `8025` |
| smtp     |`0.0.0.0`| `1025` |
