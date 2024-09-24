---
title: How to Sign Commits and Tags with GPG
date: 2018-04-30
tags:
- Git
- GPG
- Tutorial
slug: how-to-sign-commits-and-tags-with-gpg
aliases: /en/how-to-sign-commits-and-tags-with-gpg.html
---

## Why Should You Use GPG?

By default, Git will not verify your identity in each commit, which means someone can use your identity to push commits.

This may be difficult to understand. Take GitHub as an example, to push commits, you need to register your SSH keys to GitHub. And you need to be an owner or contributor of a repo to push commits (otherwise, you have to use pull request). So it seems that there is no problem related to identity.

It is not true. GitHub uses SSH Keys to identify you and let you push your commits. But Git itself does not use SSH Keys to identify you. Instead, it uses the name and email you set in Git to identity you. An example below is that my friend (Ricardo) registered his SSH Key but did not give his information to Git. As a result, he can push commits to our GitHub project. But the contributor is the name of his computer, and GitHub cannot find an account that matches the email address.

{{< figure src="images/GPG-Example.png" title="An Example" class="figure-center" >}}

<!--more-->

## How to Set Up GPG

This blog post is base on Mac. If you are using Linux or Windows, please check [GitHub Help: Signing commits with GPG](https://help.github.com/articles/signing-commits-with-gpg/).

Mac does not come with GPG by default. If you type `gpg` in Terminal and saw the error below, you have not installed GPG. Please visit [this post](https://blog.iltc.io/how-to-install-ruby-on-rails-on-mac.html "How to Install Ruby (on Rails) on Mac") and follow the first two sections to install Homebrew and GnuPG.

```bash
-bash: gpg: command not found
```

### Generating a New GPG Key

Paste the following text to the Terminal to generate a GPG key pair.

```bash
gpg --full-generate-key
```

When prompt, press `Enter` to accept the default `RSA and RSA`.

When asking key size, type `4096` instead of the default `2048`.

Enter the length of time the key should be valid. Press `Enter` to specify the default selection, indicating that the key doesn't expire.

When asking for user information, make sure you use the same email address as Github.

When asking for a password, type a secure password.

Type the command below list GPG keys for which you have both a public and private key.

```bash
gpg --list-secret-keys --keyid-format LONG
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 expires: 2017-03-10
uid						           Hubot
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

From the list of GPG keys, copy the GPG key ID. In this example above, the GPG key ID is `3AA5C34371567BD2`.

Paste the text below, change the GPG key ID to the key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`.

```bash
gpg --armor --export 3AA5C34371567BD2 | pbcopy
```

After that, the GPG Public Key will be copied to your clipboard.

### Adding the New GPG Key to GitHub

Visit [https://github.com/settings/keys](https://github.com/settings/keys "https://github.com/settings/keys") and click `New GPG key`. In the "Key" field, paste your GPG key.

### Telling Git About Your GPG Key

To set your GPG signing key in Git, paste the text below, change the GPG key ID to the key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`.

```bash
git config --global user.signingkey 3AA5C34371567BD2
```

If you aren't using the GPG suite, paste the text below to add the GPG key to your bash profile.

```bash
echo 'export GPG_TTY=$(tty)' >> /.bash_profile
```

### Signing Commits and Tags Using GPG

When committing changes in your local branch, add the `-S` flag to the git commit command and provide the passphrase you set up when you generated your GPG key.

```bash
git commit -S -m "your commit message"
```

To configure your Git client to sign commits by default for a local repository, in Git versions 2.0.0 and above, run `git config commit.gpgsign true`. To sign all commits by default in any local repository on your computer, run `git config --global commit.gpgsign true`.

### A Note

If you use IDE like me, you may get an error when you try to commit your change in the IDE:

```bash
$ git commit -S -m "Added an image"
error: gpg failed to sign the data
fatal: failed to write commit object
```

I was confused when I first encountered this situation. When I searched the error message online, I found that someone recommended to use `echo "test" | gpg --clearsign` to reproduce the error. When I typed the command, I got a result:

```bash
$ echo "test" | gpg --clearsign
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

test
gpg: sign failed: Screen or window too small
gpg: [stdin]: clear-sign failed: Screen or window too small
```

So the reason is obvious, my terminal window is too small to show the password prompt. I increased the window size and everything was fine.


