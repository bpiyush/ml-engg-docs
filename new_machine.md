---
layout: default
title: Starting on a new machine
nav_order: 3
---

# Starting on a new machine
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Basics

### Terminal

* Download [`iterm2`](https://iterm2.com/) and make it the default terminal. Go customize the color of text and background.
* Generally, default command line shell is `bash`. If it suits you, you may try `oh my zsh` and `fish`. I would recommend sticking to `bash`.

### Setting up bash profile

* Set colors on CLI for commands etc. Put the following in `~/.bash_profile`.
  ```bash
  export CLICOLOR=1
  ```

* Configure your terminal prompt. I prefer the following one which looks like the following. Put the following in `~/.bash_profile`. 

  ```bash
  parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
  }
  
  export PS1="=> \[\033[32m\] \W\[\033[34m\]\$(parse_git_branch)\[\033[00m\] $ "
  ```

<p align="center">
  <img src="assets/prompt.png" />
</p>


* See more useful examples of stuff to put in `~/.bash_profile`. Also, see [this](https://joshstaiger.org/archives/2005/07/bash_profile_vs.html) for a difference between `~/.bashrc` of Ubuntu and `~/.bash_profile` on MacOS.


