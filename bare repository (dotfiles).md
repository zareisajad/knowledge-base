
### 1. Create a bare repo

`git init --bare $HOME/.dotfiles`

### 2. Create an alias for managing it

Add this to your `~/.zshrc`:

`alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'`

Reload shell:

`source ~/.zshrc`

### 3. Tell git to ignore untracked files

```
```
d‍tfiles config --local status.showUntrackedFiles no‍‍‍‍‍

