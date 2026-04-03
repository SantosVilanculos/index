## 1. Changing Commit Messages

You can change commit messages using interactive rebase:

- Use `git rebase -i --root` to edit all commits from the root.
- Use `git rebase -i <commit_hash>` to edit a specific commit.

## 2. Removing Credentials from Git on Local Machine

### 2.1. Using Git Credential Helper

- **Clear all cached credentials**:

  ```sh
  git credential-cache exit
  git config --local --unset credential.helper
  git config --global --unset credential.helper
  git config --system --unset credential.helper
  ```

- **Remove specific credentials**: Edit the `.git-credentials` file (location depends on OS):
  - Windows: `C:\Users\<YourUsername>\.git-credentials`
  - macOS/Linux: `~/.git-credentials`

### 2.2. Using the Git Configuration File

- Open the Git configuration file:

  ```sh
  ~/.gitconfig or C:\Users\<YourUsername>\.gitconfig
  ```

- Look for the sections with credential information and remove or comment them out:

  ```sh
  [credential]
      helper = store
  [user]
      name = yourusername
      email = youremail@example.com
  ```

### 2.3. Using Credential Manager for Windows (GCM)

1. Open **Control Panel**.
2. Navigate to **User Accounts** > **Credential Manager**.
3. Under **Windows Credentials**, locate the Git-related credentials.
4. Click on the credentials and select **Remove**.

## 3. Deleting branches

### 3.1. Deleting local branches in Git

```sh
git branch -d <branch>
```

### 3.2. Deleting remote branches in Git

To delete a remote branch, we do not use the "git branch" command - but instead `git push` with the `--delete` flag:

```sh
git push origin --delete <branch>
```
## 4. Deleting tags

```sh
git tag -d $(git tag -l);
git push origin --delete $(git tag -l)
``` 
