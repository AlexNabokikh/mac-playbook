# Windows Ansible Playbook

![badge-gh-actions]
![badge-license]

This playbook installs and configures most of the software I use on my OSX machine for software development.

## Contents

* [Playbook capabilities](#playbook-capabilities)
* [Installation](#installation)
* [Running a specific set of tagged tasks](#running-a-specific-set-of-tagged-tasks)
* [Overriding Defaults](#overriding-defaults)
* [Included Applications / Configuration (Default)](#included-applications--configuration-default)

## Playbook capabilities

> **NOTE:** The Playbook is fully configurable. You can skip or reconfigure any task by [Overriding Defaults](#overriding-defaults).

* **Software**
  + Ensure software and packages selected by the user are installed via [HomeBrew](https://github.com/Homebrew/brew).
  + Ensure App Store software selected by the user are installed via [MAS](https://github.com/mas-cli/mas).
  + Ensure PIP (Python) selected packages selected by the user are installed via [MAS](https://github.com/mas-cli/mas).
* **Dotfiles**
  + Installs a set of dotfiles from a given Git repository.
* **OSX Settings**
  + **Dock**
    - Ensures items in your macOS Dock configured as you want.
  + **Fonts**
    - Ensures chosen custom fonts are installed.
  + **directories**
    - Ensures custom user directories created.
* **Terminal Settings**
  + **Sudoers**
    - Ensures custom user sudoers config applied.
  + **Vim**
    - Ensures [Vundle plugin manager](https://github.com/VundleVim/Vundle.vim) installed.
    - Ensures plugins from your .vimconfig are installed and updated.

## Installation

1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

    1. Upgrade Pip: `python -m pip install --upgrade pip`
    2. Install Ansible: `python -m pip install --user ansible`

2. Clone or download this repository to your local drive.
3. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible collections.
4. Run `ansible-playbook main.yml` inside this directory.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook` 's `--tags` flag. The tags available are `dotfiles` , `homebrew` , `mas` , `dock` , `sudoers` , `fonts` , `vundle` , `terminal` , `directories` and `osx` .

```sh
ansible-playbook main.yml -K --tags "dotfiles, homebrew"
```

## Overriding Defaults

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and enable/disable specific tasks with something like:

```yaml
---
downloads: ~/.ansible-downloads/

configure_dotfiles: true
configure_terminal: true
configure_osx: true
configure_vundle: true
create_directories: true
update_pip_packages: true

pip_executable: pip
pip_install_packages:
  - name: pip
    state: latest

# Set to 'true' to configure the Dock via dockutil.
configure_dock: true
dockitems_remove:
  - Launchpad
  - Safari
  - Messages
  - Mail
  - Maps
  - Photos
  - FaceTime
  - Calendar
  - Contacts
  - Reminders
  - Notes
  - TV
  - Music
  - Podcasts
  - App Store
  - System Preferences
  - Downloads
dockitems_persist:
  - name: Firefox
    path: /Applications/Firefox.app/
    pos: 1
  - name: Slack
    path: /Applications/Slack.app/
    pos: 2
  - name: Reeder
    path: /Applications/Reeder.app/
    pos: 3
  - name: Notion
    path: /Applications/Notion.app/
    pos: 4
  - name: Terminal
    path: /System/Applications/Utilities/Terminal.app
    pos: 5
  - name: Visual Studio Code
    path: /Applications/Visual Studio Code.app/
    pos: 6
  - name: Telegram
    path: /Applications/Telegram.app/
    pos: 7
  - name: Spotify
    path: /Applications/Spotify.app/
    pos: 8
  - name: Grammarly Editor
    path: /Applications/Grammarly Editor.app/
    pos: 9
  - name: Microsoft Remote Desktop
    path: /Applications/Microsoft Remote Desktop.app/
    pos: 10
  - name: Session
    path: /Applications/Session.app/
    pos: 11

configure_sudoers: true
sudoers_custom_config: |
  # Allow users in admin group to use sudo with no password.
  %admin ALL=(ALL) NOPASSWD: ALL

install_fonts: true
installed_nerdfonts:
  - Meslo

dotfiles_repo: https://github.com/AlexNabokikh/dotfiles
dotfiles_repo_accept_hostkey: true
dotfiles_repo_local_destination: ~/Documents/repositories/dotfiles
dotfiles_files:
  - .gitconfig
  - .inputrc
  - .osx
  - .p10k.zsh
  - .tmux.conf
  - .vimrc
  - .zshrc

homebrew_installed_packages:
  # - aws-vault
  # - awscli
  - bat
  - dive
  # - eksctl
  - fzf
  - git
  - gpg
  - helm
  - htop
  - java11
  - jq
  - krew
  - kube-linter
  - kubernetes-cli
  - minikube
  - mtr
  - nmap
  - pidof
  - pinentry
  # - poetry
  # - pre-commit
  # - pyenv
  - reattach-to-user-namespace
  - telnet
  - shellcheck
  - shfmt
  - terminal-notifier
  - terraform-docs
  - terraforming
  - terragrunt
  - tfenv
  - tflint
  - tmux
  - tree
  - vim
  - watch
  - wget

homebrew_taps:
  - aws/tap
  - domt4/autoupdate
  - homebrew/bundle
  - homebrew/cask
  - homebrew/cask-drivers
  - homebrew/cask-versions
  - homebrew/core
  - homebrew/services
  - romkatv/powerlevel10k

homebrew_cask_appdir: /Applications
homebrew_cask_apps:
  - appcleaner
  - bitwarden
  - docker
  - dozer
  - firefox
  - focus
  # - google-chrome
  - grammarly
  - itsycal
  - krisp
  # - logitech-g-hub
  - logitech-options
  - microsoft-remote-desktop
  - mos
  - nightowl
  - notion
  - onyx
  - reaper
  # - slack
  - spotify
  - telegram
  - visual-studio-code
  - vlc
  - zoom

mas_installed_apps:
  - {id: 441258766, name: Magnet}
  - {id: 937984704, name: Amphetamine}
  - {id: 984968384, name: Redacted}
  - {id: 1020812363, name: CopyClip 2 - Clipboard Manager}
  - {id: 1521432881, name: Session - Pomodoro Focus Timer}
  - {id: 1528890965, name: TextSniper - OCR simplified}
  - {id: 1529448980, name: Reeder 5.}
mas_email: ""
mas_password: ""

osx_script: ~/.osx --no-restart

# Glob pattern to ansible task files to run after all other tasks are finished.
post_provision_tasks: []
```

## Included Applications / Configuration (Default)

Packages (installed with Chocolatey):

* 7zip
* adobereader
* auto-dark-mode
* awscli
* capture2text
* choco-cleaner
* choco-upgrade-all-at-startup
* Firefox
* git
* golang
* jre8
* kubernetes-cli
* microsoft-windows-terminal
* powertoys
* python3
* telegram
* terraform
* vlc
* vscode
* zoom

## Author

This project was created by [Alexander Nabokikh](https://www.linkedin.com/in/nabokih/) (originally forked from Jeff Geerling's repository: [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)).

## License

This software is available under the following licenses:

* **[MIT](https://github.com/AlexNabokikh/mac-playbook/blob/master/LICENSE)**

[badge-gh-actions]: https://github.com/AlexNabokikh/windows-playbook/actions/workflows/release.yaml/badge.svg
[badge-license]: https://img.shields.io/badge/License-MIT-informational
