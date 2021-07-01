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

## Setting up the terminal

### Basics

* Download [`iterm2`](https://iterm2.com/) and make it the default terminal
* Generally, default command line shell is `bash`. If it suits you, you may try `oh my zsh` and `fish`. I would recommend sticking to `bash`.
* Configure your terminal prompt. I prefer the following one which looks like the following:

![prompt](assets/prompt.png)

```bash
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

export PS1="=> \[\033[32m\] \W\[\033[34m\]\$(parse_git_branch)\[\033[00m\] $ "
```

