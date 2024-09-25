---
title: How to Install Ruby (on Rails) on Mac
date: 2018-01-22
tags:
- Language
- Tutorial
- Ruby
- Ruby on Rails
aliases: /en/how-to-install-ruby-on-rails-on-mac.html
---

macOS already included Ruby. You can type `ruby -v` at a Terminal prompt to check Ruby version and see result like:

```
ruby 2.3.3p222 (2016-11-21 revision 56859) [universal.x86_64-darwin17]
```

If you are satisfied with the Ruby version, you can type `gem install rails --no-ri --no-rdoc` to install Rails.

However, if you are not satisfied with the Ruby version or if you have multiple apps that run on different Ruby version, I would recommend you use RVM to manage Ruby.

<!--more-->

## Install Homebrew

[Homwbrew](https://brew.sh/) is a package manager for macOS. It can install the stuff you need that Apple does not have. Type the following code at a Terminal prompt to install Homebrew.

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

After the installation, remember to close and re-open your terminal.

## Install GnuPG

GnuPG (with binary name gpg) is an application used for public key encryption using the OpenPGP protocol, but also verification of signatures. We have to install GnuPG since the installation of RVM needs to add some public keys of RVM to our computer.

Type `brew install gnupg gnupg2` at a Terminal prompt to install GnuPG.

## Install RVM

[RVM](https://rvm.io/) or Ruby Version Manager is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments from interpreters to sets of gems.

Type the following code at a Terminal prompt to add gpg.

```
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

Then type `\curl -sSL https://get.rvm.io | bash -s stable` to install RVM.

After the installation, remember to close and re-open your terminal.

## Install Ruby

Now type `rvm use ruby --install --default` at a Terminal prompt to install Ruby. It may take a while so it’s good time to get a cup of tea.

After the installation, use `ruby -v` to check Ruby vision. You should see something like this:

```
ruby 2.4.1p111 (2017-03-22 revision 58053) [x86_64-darwin17]
```

## Install Rails

Now you can type `gem install rails --no-ri --no-rdoc` to install Rails.

## References

[https://stackoverflow.com/questions/27041885/how-to-resolve-gpg-command-not-found-error-during-rvm-installation](https://stackoverflow.com/questions/27041885/how-to-resolve-gpg-command-not-found-error-during-rvm-installation)