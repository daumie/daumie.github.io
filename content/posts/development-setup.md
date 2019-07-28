---
title: "MacOS Setup for Software Development"
date: 2019-07-06T02:46:32+03:00
draft: false
toc: false
images:
tags: 
  - unix
  - macos
  - development
  - git
  - gpg
  - encryption
---

## First Things First

After first login, run Software Update and ensure that the operating system is at the latest point of release. This will ensure that you have a more secure OS. Also since macOS upgrades are free, you might as well keep your machine up to date.

> To do that go: Apple menu () > About This Mac > Software Update.

Restart the computer when prompted to.

The default account created is the admin account. If other people will be using the machine, create Standard accounts as necessary.

> Admin accounts have sudo privileges: All Admin accounts on a Mac may use sudo to run command-line utilities with administrative (root) privileges.

### Enable File Vault

FileVault is a disk encryption program in Mac OS X 10.3 and later. It performs on-the-fly encryption with volumes on Mac computers.

+ [disk encryption program](https://en.wikipedia.org/wiki/Disk_encryption_software) - Disk encryption is a technology which protects information by converting it into unreadable code that cannot be deciphered easily by unauthorized people. Disk encryption uses disk encryption software like Apple's File Vault 2.

+ [volumes](https://en.wikipedia.org/wiki/Volume_(computing)) - a volume or logical drive is a single accessible storage area with a single file system, typically (though not necessarily) resident on a single partition of a hard disk. Although a volume might be different from a physical disk drive, it can still be accessed with an operating system's logical interface. However, a volume differs from a partition.

### Set a Firmware Password

A firmware password prevents starting up from any internal or external storage device other than the startup disk you've selected.
[Apple Knowledge Base article HT204455](https://support.apple.com/en-gb/HT204455) has full details on how to go around this

If you did not enable File Vault, then the attacker will have complete access to all of the files on the system.

## Setting Up for Development

The first step is to install a compiler. The easiest way to install one is with the Xcode Command Line Tools package.
Xcode is an integrated development environment for macOS containing a suite of software development tools developed by Apple for developing software for macOS, iOS, watchOS and tvOS.
Once you have the compiler that is provided by Xcode, you can use Homebrew to install everything else that you need.

From the Terminal App run:

```bash
$ xcode-select --install
.......
```

It'll prompt you to install the command line tools. Follow the instructions and you'll have Xcode and Xcode command line tools both installed.

## Setting up Homebrew

[Homebrew](http://brew.sh/) is a free and open-source software package management system that simplifies the installation of software on Apple's macOS operating system and Linux. The name is intended to suggest the idea of building software on the Mac depending on the user's taste.
Follow the instructions on the site.

You should also amend your PATH, so that the versions of tools that are installed with Homebrew take precedence over others. To do this, edit the file .bashrc in your home directory to include this line:

```bash
export PATH="/usr/local/bin:/usr/local/sbin:~/bin:$PATH"
```

To check that Homebrew is installed correctly, run this command in a terminal window:

```bash
$ brew doctor
Your system is ready to brew.
```

To update the index of available packages, run this command in a terminal window:

```bash
brew update
```

You can now use the brew install command to add command-line software to your Mac, and brew cask install to add graphical software. For example, this command installs the Slack app:

```bash
brew cask install slack
```

## Installing and Configuring Git

Git is a software that is used for Version Control. It is free and open source. Version Control is the management of changes to documents, computer programs, large websites and other collection of information.

Xcode Command Line Tools include a copy of Git but it's mostly an outdated version. To install a newer version. Run:

```bash
brew install git
```

Set your details before creating or cloning repositories. 

```bash
git config --global user.name "Your Name"
git config --global user.email "you@your-domain.com"
```

The _global_ option means that the setting will apply to every repository that you work with in the current user account.

You can create a global gitignore in your home directory `~/.gitignore`  and use it like:

```bash
git config --global core.excludesfile '~/.gitignore'
```

The editor used to edit the commit log message will be chosen from the `GIT_EDITOR` environment variable, the `core.editor` configuration variable, the `VISUAL` environment variable, or the `EDITOR environment` variable *(in that order)*.

To use `vim` as your editor, run:

```bash
git config --global core.editor "vim"
```

There's a way to skip typing  username and/password everytime when using HTTPS on Git. To have this configuration, run:

```bash
git config — global credential.helper osxkeychain
```

### Signing Commits

When you sign a Git commit, you can prove that the code you submitted came from you and wasn't altered while you were transferring it. You also can prove that you submitted the code and not someone else.

You can protect your code commits from malicious changes by GPG-signing them. GPG stand for _GNU Privacy Guard_

The simplest way of getting GPG is by downloading and installing the [GPG suite for Macos](https://gpgtools.org/)

Confirm that you have GPG installed by running this on your terminal:

```bash
$ which gpg
usr/local/bin/gpg
```

Use the following [steps to sign every commit with GPG](https://help.github.com/en/articles/generating-a-new-gpg-key).

[Add the GPG key to your GitHub account.](https://help.github.com/en/articles/adding-a-new-gpg-key-to-your-github-account)

## Text Editors

If you do not have a preferred editor, consider using a version of [Visual Studio Code](https://code.visualstudio.com/)
To work with a modern Vim editor, install [Neovim](https://neovim.io/)

Remember to setup the `EDITOR`  environment variable in your ~/.bashrc file, so that this editor is automatically invoked by command-line tools like your version control system like git as discussed before.

### TODO

+ [ ] ssh setup instructions
+ [ ] `python` development setup ( Hello World using Docker too)
+ [ ] `go` development setup
+ [ ] `node` development setup
+ [ ] `kubernetes` setup ( includes `helm`, `minikube` and `docker`)
+ [ ] `VS Code` configuration ( custom themes, fonts and plugins)
+ [ ] `neovim` setup ( fonts, plugins e.t.c)
