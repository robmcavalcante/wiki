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
git config --global --edit
```
```bash
git config --global user.name "Robson Machado Cavalcante" &&
git config --global user.email "robmcavalcante@gmail.com"

mkdir /home/$USER/.ssh && cd ~/.ssh &&
eval "$(ssh-agent -s)"

ssh-keygen -t rsa -b 4096 -C "robmcavalcante@gmail.com"

ssh-add ~/.ssh/id_rsa

# autocomplete names and branchs - git
curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```
```
Host github.com
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
   
Host ###
  User git
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
```
```bash
[user]
	name = Robson Machado Cavalcante
	email = robmcavalcante@gmail.com
[core]
	editor = vim
[alias]
	cm = commit -m
	st = status

	lg = log --all --oneline --decorate
	lgg = log --all --oneline --graph --decorate
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
```
sudo pacman -S git base-devel python-pip npm nodejs rust --needed

bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/master/utils/installer/install.sh)

export PATH~/.cargo/bin:~/.local/bin:$PATH
```

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
vscodium --install-extension abusaidm.html-snippets
vscodium --install-extension aliariff.vscode-erb-beautify
vscodium --install-extension CoenraadS.bracket-pair-colorizer-2
vscodium --install-extension eamodio.gitlens
vscodium --install-extension emmanuelbeziat.vscode-great-icons
vscodium --install-extension enkia.tokyo-night
vscodium --install-extension GitHub.github-vscode-theme
vscodium --install-extension HookyQR.beautify
vscodium --install-extension IBM.output-colorizer
vscodium --install-extension mikestead.dotenv
vscodium --install-extension ms-azuretools.vscode-docker
vscodium --install-extension MS-CEINTL.vscode-language-pack-pt-BR
vscodium --install-extension PKief.material-icon-theme
vscodium --install-extension rebornix.ruby
vscodium --install-extension vincaslt.highlight-matching-tag
vscodium --install-extension vortizhe.simple-ruby-erb
vscodium --install-extension wingrunr21.vscode-ruby
```
```yml
{
  "editor.fontSize": 13, 
  "editor.fontFamily": "'Fira Code', Consolas",
  "terminal.integrated.fontFamily": "MesloLGS NF",
  "editor.fontLigatures": true,
  "editor.rulers": [80,120],
  "editor.renderLineHighlight": "gutter",
  "terminal.integrated.fontSize": 16,
  "editor.minimap.enabled": false,
  "editor.tabSize": 2,
  "editor.lineHeight": 26,
  "editor.wordWrap": "off",
  "workbench.iconTheme": "material-icon-theme",
  "tabnine.experimentalAutoImports": true,
  "files.autoSave": "onFocusChange",

  "git.suggestSmartCommit": false,
  "json.schemas": [

  ],

  "launch": {
    "configurations": [],
    "compounds": []
  },

  "files.associations": {
    "*.html.erb": "erb"
  },

  "[erb]": {
    "editor.defaultFormatter":"aliariff.vscode-erb-beautify",
  },

  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features",
  },

  "emmet.includeLanguages": {
    "erb": "html"
  },
  
  "emmet.showAbbreviationSuggestions": true,
  "emmet.showSuggestionsAsSnippets": true,
  "terminal.integrated.tabs.location": "left",

  "editor.occurrencesHighlight": false,
  "editor.selectionHighlight": false,
  "editor.matchBrackets": "never",
  "editor.mouseWheelZoom": true,
  "workbench.colorTheme": "Tokyo Night",
  "bracket-pair-colorizer-2.depreciation-notice": false,
  "window.zoomLevel": 1,

  "solargraph.transport": "stdio",
  "solargraph.formatting": true,
  "solargraph.diagnostics": true
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

