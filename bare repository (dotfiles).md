
### 1. Create a bare repo

```zsh
git init --bare $HOME/.dotfiles`
```
### 2. Create an alias for managing it

Add this to your `~/.zshrc`:

```zsh
alias dtf='/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'
```
### 3. Tell git to ignore untracked files

```zsh
dtf config --local status.showUntrackedFiles no‍‍‍‍‍
```
#### Add your configs

```
dtf add ~/.config/i3
dtf add ~/.config/nvim 
dtf add ~/.zshrc 

dtf commit -m "Initial dotfiles"
```
#### push to github

```zsh
dtf remote add origin git@github.com:YOUR_USERNAME/dotfiles.git

dtf push -u origin master
```
Also add this alias (optional)
```zsh
alias dtfpush='dtf add -f ~/.zshrc ~/.config/i3 ~/.config/nvim && dtf commit -m "update dotfiles" && dtf push'
```
now you can simply run dtfpush and there you have all your configs on github at anytime.