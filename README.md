The default Raspberry Pi OS image provided by the Raspberry Pi Foundation is based on age-old Debian GNU/Linux traditions. These, however, are often neither secure nor good-looking. This guide shows you how to beautify and secure your setup.

## About this Guide

The guide is divided into three sections: [Steps for the desktop version of Raspberry Pi OS](#steps-for-the-desktop-version-of-raspberry-pi-os), [Steps for Raspberry Pi OS Lite](#steps-for-raspberry-pi-os-lite) and [Steps for both versions of Raspberry Pi OS](#steps-for-both-versions-of-raspberry-pi-os).
All instructions refer to the latest version of Raspberry Pi OS (stable). Currently, this version is called Bullseye, while the legacy version (oldstable) is called Buster.

Some links on this page link to external pages. These pages contained useful information at the time of writing, but may or may not have changed drastically since I last checked. I can and do not take any responsiblity for the content of those pages.

## Steps for the desktop version of Raspberry Pi OS

### Software

#### Install a graphical package manager

While Raspberry Pi OS has the Recommended Software tool and in older versions the Add/Remove Programs utility, they are not very good. Recommended Software only has a few programs that are preinstalled on the full image anyways and Add/Remove Programs' UI is so terrible, that I'm not even listing it as an option to install software.

##### GNOME Software

If you have previously used Ubuntu or any derivative like Vanilla OS, you might now the Software application (known as `snap-store` internally). But did you know, that it is a fork of GNOME Software, an application you can actually install on Raspberry Pi OS? GNOME Software was also used in versions of Ubuntu prior to 20.04 LTS, but was replaced by `snap-store` in that version to push people into using Canonical's proprietary `snap` format.

To install GNOME Software, run this command in a terminal window:

```bash
sudo apt install gnome-software
```

If you installed alternative package managers like Flatpak and Snap, you might also want the appropriate plugins:

```bash
sudo apt install gnome-software-plugin-flatpak
sudo apt install gnome-software-plugin-snap
```

##### Pi-Apps

Another graphical package manager I use is Botspot's [Pi-Apps][pi-apps].

Pi-Apps is more of a collection of installation scripts than a real package manager, but is useful to install apps that either only provide binary files (like [Oh My Posh](#oh-my-posh)) or use a custom APT server (like `nodejs`).

Install Pi-Apps using the official install script:

```bash
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/install | bash
```

Most of the tools on this list can be installed through Pi-Apps.

#### Install the LibreOffice suite.

LibreOffice is a set of programs much like Microsoft Office. It has a word processor, spreadsheet program, presentation creator and others.

LibreOffice is available from APT, GNOME Software, Raspberry Pi Recommended Software and Pi-Apps.

To install it from APT:

```bash
sudo apt install libreoffice
```

### Install Visual Studio Code

If you plan on editing text files, Microsoft's [Visual Studio Code](https://code.visualstudio.com/) is a must. Raspberry Pi OS's included Mousepad and Geany editors aren't fit for editing large codebases or using programs like Git.

Visual Studio Code is available on APT , Pi-Apps and Recommended Software. While it also shows up on GNOME Software if you have Flatpak installed, that version is not official and has many limitations, such as not being able to communicate with other programs. There is also a `.deb` archive available for download on it's website.

To install from APT:

```bash
sudo apt install code
```

### Install another web browser

The default web browser application that comes with Raspberry Pi OS is Chromium, a stripped-down, open-source version of Google Chrome. It takes forever to start up and pressures you into using a Google Account. While your favorite web browser might offer a Linux download (no, Microsoft Edge (Chromium) fans, you don't even need to ask), a good choice is [Mozilla Firefox].

Firefox ESR is available on APT, while Rapid Release versions, which update every six weeks are not. These can be installed using Pi-Apps.
It is also available from Flatpak, and, by extension (great pun), GNOME Software.

### Install an E-Mail program

Raspberry Pi OS (full) comes with the Claws Mail client. Another great mail client is [Mozilla Thunderbird](https://thunderbird.net), which also supports GnuPG (see ["Encrypt your email"](#encrypt-your-email))

### Other things to do

#### Encrypt your email

As a Debian GNU/Linux system, the Raspberry Pi is the perfect starting point for GnuPG email encryption.
The Free Software Foundation provides an excellent [beginner's guide](https://emailselfdefense.fsf.org).

#### Customize your launch bar

When right-clicking any part of the launch bar at the top of the screen, a menu containing the elements "\<element> Settings" and "Add/Remove bar elements" appears.
The settings for the app launch bar are especially relevant, as the allow you to add custom programs to it.

#### Read Raspberry Pi Press

The Bookshelf application allows you to download and read PDFs of Raspberry Pi magazines and books completely for free.
You can find it preinstalled in the start menu: Help &rightarrow; Bookshelf.

#### Add custom entries to the start menu

All start menu entries are simple `.desktop` files saved in specific locations (`/usr/share/applications/`, `/usr/local/share/applications/` and `~/.local/applications/` specifically), which you can edit and remove, or add new ones if you wish. There are many graphical generators for these files online.

## Steps for Raspberry Pi OS Lite

### Software

Such as with the desktop version of Raspberry Pi OS, there are many great pieces of software for Raspberry Pi OS Lite.

While you will need to install most of these using the command line, a command line extension for Pi-Apps exists, which you will need to install manually

#### Install Pi-Apps and the Pi-Apps command line extension

While the command line extension to [Pi-Apps][pi-apps] is available on Pi-Apps itself, you cannot use the GUI and have to install it yourself:

```bash
cd
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/install | bash
wget https://raw.githubusercontent.com/Itai-Nelken/PiApps-terminal_bash-edition/main/pi-apps-terminal-bash-edition.sh
ln -s /home/YOURNAME/pi-apps-terminal-bash-edition.sh ~/bin/pi-apps
```

#### Install the Lynx browser

Lynx is one of the oldest, still maintained browsers, and runs fully in your terminal.

Install with APT:

```bash
sudo apt install lynx
```

#### Install `tldr`

Most Linux and Unix users should be familiar with `man`: a database of the manuals for almost all installed programs, configuration files and more.

[TLDR](https://tldr.sh) takes a different approach. Instead of documenting everything a program has to offer, tldr pages show only the most common commands, and are similar to most programs' `--help` output (but readable).

The official tldr client is installed with npm, but another client, [tealdeer](https://github.com/dbrgn/tealdeer), is available on Pi-Apps.

## Steps for both versions of Raspberry Pi OS

### Software

#### Install Flatpak

Flatpak is a package manager that installs all software in separate sandboxes. While this sometimes leads to limitations like with VSCode ([see above](#install-visual-studio-code)), it makes your system way more secure.

To install Flatpak, run these commands:

```bash
sudo apt install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

If you installed [GNOME Software](#gnome-software), you probably want to install the Flatpak plugin too:

```bash
sudo apt install gnome-software-plugin-flatpak
```

#### Use a better shell

The default shell preinstalled on Raspberry Pi OS is `bash`. Bash is a very old shell, directly based on the original Unix `sh`, and should be replaced with a more modern shell like `zsh`, `fish` or PowerShell. Installation instructions can be found on the respective shell's website, and PowerShell is available on Pi-Apps.

After installing the shell, use this command to make it your default shell:

```bash
chsh -s $(which zsh)
```
replacing `zsh` with your new shell. Note that you need to transfer any changes to `~/.bashrc`, `~/.bash_profile` and `/etc/bash_completion.d/` to your new shell's profile.

#### Activate SSH access

While SSH can be a security problem, especially with weak passwords, it can be a useful tool, especially on systems running Pi OS Lite. This problem can also be eliminated by using key authentication (see `man ssh-keygen` on your client computer).

To enable the SSH server, activate option I2 (Interface &rightarrow; SSH) in `raspi-config`.

### Security changes

The default configuration of Raspberry Pi OS is very insecure. This includes problems like

- public home directories
- the OOBE user not needing a password for sudo
- autologin

These can thankfully be changes easily:

#### Disabling Autologin

To disable Autologin, simply open the Raspberry Pi Configuration (`raspi-config`) tool and go to System Options &rightarrow; Boot / Autologin. Disable this option.

#### Requiring a password for sudo

This is also a simple change: you simply need to remove the file `/etc/sudoers.d/010_pi-nopasswd` as root.

#### Making home directories private

This change is not as simple, but still easy.

To make the home directory of new users private, edit `/etc/login.defs` as root (for example, using `sudo geany /etc/login.defs`) and replace the line

```defs
UMASK 022
```

with

```defs
UMASK 077
```

To make existing user's home directories private, run

```bash
sudo chmod 700 /home/*
```

### Other configuration changes

If you have a Pi 4 with it's [case](https://www.raspberrypi.com/products/raspberry-pi-4-case/), it may be worth checking out the [official case fan](https://www.raspberrypi.com/products/raspberry-pi-4-case-fan/) (after you have drilled holes into the lid to help ventilation).

You can cofigure it with `raspi-config` (option P4). This requires a reboot.

[pi-apps]: https://github.com/Botspot/pi-apps
