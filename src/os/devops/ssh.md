```sh
mkdir -p "$HOME/.ssh"
cd "$HOME/.ssh"

ssh-keygen -t ed25519 -C <email>

eval "$(ssh-agent -s)"
ssh-add $HOME/.ssh/<path>
```
