---
title: Programming Setup
author: robson
date: 2023-02-21 12:16:00 +0800
categories: [setup]
tags: [git, database, docker, lunarvim, rubyonrails, virtualbox, vagrant, vscode, zsh]
pin: true
---

# Programming Setup

## GIT
```bash
echo "[user]
	name = Robson Machado Cavalcante
	email = robmcavalcante@gmail.com
[core]
	editor = vim
[alias]
	cm = commit -m
	st = status

	lg = log --all --oneline --decorate
	lgg = log --all --oneline --graph --decorate" > /home/robson/.gitconfig
```
{: file='.gitconfig'}


```bash
mkdir /home/$USER/.ssh && cd ~/.ssh &&
eval "$(ssh-agent -s)"

ssh-keygen -t rsa -b 4096 -C "robmcavalcante@gmail.com"

ssh-add ~/.ssh/id_rsa
```

---

```bash
Host github.com
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
   
Host gitlab.com
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
```
{: file='config'}

```bash
# autocomplete names and branchs - git
curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

## DATABASE
```bash
pacman -S nodejs rbenv sqlite sqlite3 libsqlite3-dev postgresql
```

## DOCKER
```bash
pacman -S docker
systemctl enable docker
systemctl start docker
usermod -a -G docker $USER
reboot
```
```
docker run hello-world
```

## LUNARVIM
```bash
sudo pacman -Syyuu vim neovim && yay -S gnome-terminal-transparency
```
```bash
sudo pacman -S git base-devel python-pip npm nodejs rust --needed

bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/master/utils/installer/install.sh)

export PATH~/.cargo/bin:~/.local/bin:$PATH
```

```lua
-- vim options
vim.opt.shiftwidth = 2
vim.opt.tabstop = 2

-- general
lvim.log.level = "info"
lvim.format_on_save = {
  enabled = true,
  pattern = "*.lua",
  timeout = 1000,
}

lvim.leader = "space"

lvim.keys.normal_mode["<C-s>"] = ":w<cr>"

lvim.builtin.alpha.active = true
lvim.builtin.alpha.mode = "dashboard"
lvim.builtin.terminal.active = true
lvim.builtin.nvimtree.setup.view.side = "left"
lvim.builtin.nvimtree.setup.renderer.icons.show.git = false

lvim.builtin.treesitter.auto_install = true
```
{: file='config.lua'}

## RUBYONRAILS
```bash
yay -S rbenv ruby-build
yay --clean
rm -rf ~/.rbenv
yay -S --rebuildall --redownload --noconfirm rbenv ruby-build

eval "$(rbenv init -)"

rbenv install 3.1.2
rbenv global 3.1.2
gem install openssl
```

## VIRTUALBOX
```bash
pacman -Syyuu virtualbox virtualbox-host-modules-arch &&
sudo modprobe vboxdrv &&
sudo vboxreload
```

## VAGRANT
```bash
pacman -S vagrant
vagrant plugin install vagrant-vbguest
```

## VSCODE
```bash
  yay -S ttf-meslo-nerd-font-powerlevel10k nerd-fonts-meslo
```
```bash


vscodium --install-extension eamodio.gitlens
vscodium --install-extension enkia.tokyo-night

vscodium --install-extension HookyQR.beautify

vscodium --install-extension PKief.material-icon-theme
vscodium --install-extension vincaslt.highlight-matching-tag
vscodium --install-extension vortizhe.simple-ruby-erb
vscodium --install-extension wingrunr21.vscode-ruby
```
```yml
{
    "json.schemas": [
    ],

    // emmet consider .rb files as html
    "emmet.includeLanguages": {
        "erb": "html"
    },

    // disable minimap
    "editor.minimap.enabled": false,

    // limiting vertical rulers
    "editor.rulers": [80,120],

    // Font, line height, theme and icons
    "editor.fontSize": 14,
    "editor.fontFamily": "Consolas",
    "editor.lineHeight": 24,
    "workbench.colorTheme": "Tokyo Night",
    "workbench.iconTheme": "material-icon-theme",

    // save file when changing focus
    "files.autoSave": "onFocusChange",

    // do not show all tags the same
    "editor.occurrencesHighlight": false,

    // location of terminal tabs
    "terminal.integrated.tabs.location": "left",

    //  dont't match brackets
    "editor.matchBrackets": "never",

    // enable zoom on mouse scroll
    "editor.mouseWheelZoom": true,
}
```
{: file='settings.yml'}

## ZSH
```bash 
yay -S ttf-meslo-nerd-font-powerlevel10k
```

```bash
pacman -S zsh && chsh -s $(which zsh)
```
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k &&
touch ~/.zshrc &&
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
```
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions &&
echo 'source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh' >> ~/.zshrc
```
```bash
touch ~/.zshrc &&

echo 'SAVEHIST=1000  # Save most-recent 1000 lines
HISTFILE=~/.zsh_history' >> ~/.zshrc
```

## .zshrc
```bash
# ZSH Config
source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh

# Folders
alias dev='cd /home/robson/.development'

# VS Codium
alias code='vscodium'

# RubyOnRails
alias rails='bundle exec rails'
alias importmap='./bin/importmap'
alias rs='bundle exec rails s'
alias rc='bundle exec rails c'
alias raps='bundle exec rails assets:precompile && bundle exec rails s'

eval "$(rbenv init -)"
```
{: file='.zshrc'}

## .zshenv
```bash 
# Oracle Client
export LD_LIBRARY_PATH=/opt/oracle/instantclient_21_6:$LD_LIBRARY_PATH
export PATH=/opt/oracle/instantclient_21_6:$PATH

# LVIM
export PATH=/home/robson/.local/bin:$PATH
```
{: file='.zshenv'}

## .gitconfig
```bash
[user]
	email = robmcavalcante@gmail.com
	username = Robson Machado Cavalcante
[core]
	editor = vim
[alias]
	cm = commit -m
	st = status
	lg = log --oneline --color
```
{: file='.gitconfig'}

