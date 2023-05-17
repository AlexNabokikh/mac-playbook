# macOS Ansible Playbook

![badge-gh-tests]
![badge-gh-release]
![badge-license]

This playbook installs and configures most of the software I use on my OSX machine for software development.

## Contents

- [Playbook capabilities](#playbook-capabilities)
- [Installation](#installation)
- [Running a specific set of tagged tasks](#running-a-specific-set-of-tagged-tasks)
- [Overriding Defaults](#overriding-defaults)

## Playbook capabilities

> **NOTE:** The Playbook is fully configurable. You can skip or reconfigure any task by [Overriding Defaults](#overriding-defaults).

- **Software**
  - Install software and packages selected by the user via [Homebrew](https://github.com/Homebrew/brew).
  - Install App Store software selected by the user via [MAS](https://github.com/mas-cli/mas).
  - Install PIP (Python) packages selected by the user.
  - Install NPM (JS) packages selected by the user.
- **Dotfiles**
  - Install a set of dotfiles from a given Git repository.
- **System Settings**
  - **Dock**
    - Arrange items in your macOS Dock as you want.
  - **Fonts**
    - Install chosen custom fonts.
  - **directories**
    - Create custom user directories.
- **Terminal Settings**
  - **Sudoers**
    - Configure custom sudoers.
  - **Zsh**
    - Install and configure [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh).
    - Install custom OMZ plugins and themes.
  - **TMUX**
    - Install and configure tmux with TPM (plugin manager)

## Installation

1. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

   1. Upgrade Pip: `python -m pip install --upgrade pip`
   2. Install Ansible: `python -m pip install --user ansible`

2. Clone or download this repository to your local drive.
3. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible collections.
4. Run `ansible-playbook main.yml` inside this directory.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook` 's `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `dock`, `sudoers`, `fonts`, `vim`, `terminal`, `directories`, and `osx` .

```sh
ansible-playbook main.yml -K --tags "dotfiles, homebrew"
```

## Overriding Defaults

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and enable/disable specific tasks with something like:

```yaml
---
create_directories: true
directories:
  - $HOME/Documents/repositories

pip_executable: pip
pip_packages:
  - name: mkdocs

npm_packages:
  - name: prettier
    state: latest

configure_dock: true
dockitems_remove:
  - TV

dockitems_persist:
  - name: Firefox
    path: /Applications/Firefox.app/
    pos: 1
  - name: Terminal
    path: /System/Applications/Utilities/Terminal.app
    pos: 2
  - name: Visual Studio Code
    path: /Applications/Visual Studio Code.app/
    pos: 3

install_fonts: true
installed_nerdfonts:
  - Meslo

homebrew_installed_packages:
  - git

homebrew_cask_appdir: /Applications
homebrew_cask_apps:
  - firefox
  - visual-studio-code

mas_installed_apps:
  - { id: 937984704, name: Amphetamine }
  - { id: 984968384, name: Redacted }
mas_email: ""
mas_password: ""
```

## Author

This project was created by [Alexander Nabokikh](https://www.linkedin.com/in/nabokih/) (originally forked from Jeff Geerling's repository: [geerlingguy/mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook)).

## License

This software is available under the following licenses:

- **[MIT](https://github.com/AlexNabokikh/mac-playbook/blob/master/LICENSE)**

[badge-gh-tests]: https://github.com/AlexNabokikh/mac-playbook/actions/workflows/ci.yml/badge.svg
[badge-gh-release]: https://github.com/AlexNabokikh/mac-playbook/actions/workflows/release.yaml/badge.svg
[badge-license]: https://img.shields.io/badge/License-MIT-informational
