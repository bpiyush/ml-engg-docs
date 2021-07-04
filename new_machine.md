---
layout: default
title: Starting on a new machine
nav_order: 2
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

* Set command autofill: Check [this](https://sourabhbajaj.com/mac-setup/BashCompletion/)
  out for bash autofill and suggestions.

* Set global enviornment variables like the following (depending on what is relevant)
  ```bash
  PATH=$PATH:/usr/local/; export PATH
  PATH=$PATH:/Users/piyushbagad/Library/; export PATH
  export LDFLAGS="-L/usr/local/opt/gettext/lib"
  export CPPFLAGS="-I/usr/local/opt/gettext/include"
  export PATH="/usr/local/opt/:$PATH"
  export PATH="$HOME/.cargo/bin:$PATH"
  ```

## Setting up bash aliases & others

You can pick and choose from the following to put in `~/.bash_aliases`.
Don't forget to source it in `~/.bash_profile` though using.
```bash
source ~/.bash_aliases
```

```bash
# <----------- Basic aliases --------->
  alias c='clear'
  alias q='exit'
  alias home='cd ~'
  alias root='cd /'
  alias dtop='cd ~/Desktop'
  alias cd..='cd ../'
  alias ..='cd ../'
  alias ...='cd ../../'
  alias .3='cd ../../../'
  alias .4='cd ../../../../'
  alias .5='cd ../../../../../'
  alias .6='cd ../../../../../../'

# <---------- Text colors ------------>
RED=$(tput setaf 1)
GREEN=$(tput setaf 2)
YELLOW=$(tput setaf 3)
BLUE=$(tput setaf 4)
ORANGE=$(tput setaf 5)
CYAN=$(tput setaf 6)
WHITE=$(tput setaf 7)
BLACK=$(tput setaf 8)
REMOVE_ALL=$(tput sgr0)

# <---------- Git helpers ------------>
alias gc='git checkout'
alias pull='git pull'
alias gs='git status'
alias gd='git diff'
alias glog="git log"
alias glogv="git log --graph --oneline --decorate"
alias gba='git branch -a'
alias git_visualize="git log --graph --oneline --decorate"

function rm_files () {
        find . -name $1 -exec rm -rf {} \;
}

alias reset-local='git add .; git stash; pull'

function parse_git_branch {
	git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
function parse_git_branch_raw {
	git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'
}

function commit () {
        git commit -m "$1";
        echo $RED"=> Pushing to branch "$YELLOW$(parse_git_branch_raw)$REMOVE_ALL
        git push origin $(parse_git_branch_raw);
}

function new_branch () {
        if [ -z "$(git status --porcelain)" ]; then
                # Working directory clean
                echo "$CYAN => Clean working directory. Creating branch $YELLOW $1 $REMOVE_ALL"
                git checkout -b $1
                git push origin -u $1
        else
                # Uncommitted changes
                echo "$RED You have uncommitted changed. Please check! $REMOVE_ALL"
        fi
}

function rm_local_branch () {
        echo "$RED => Deleting local branch $YELLOW $1  $REMOVE_ALL"
        git branch -D $1
}

function rm_remote_branch () {
        echo "$RED => Deleting remote branch $YELLOW $1 $REMOVE_ALL"
        git push origin --delete $1
}

function rm_local_and_remote_branch () {
        rm_local_branch "$1"
        rm_remote_branch "$1"
}

function rename_branch () {
        oldname=$1
        newname=$2
        currbranch=$(parse_git_branch_raw)

        echo "$BLUE => Current working branch: $YELLOW $currbranch $REMOVE_ALL"
        echo "$BLUE => Renaming branch $YELLOW $oldname $REMOVE_ALL to $YELLOW $newname $REMOVE_ALL"

        if ["$oldname" == "$currbranch"]
        then
                git branch -m "$newname";
        else
                git branch -m "$oldname" "$newname"
        fi

        git push origin :"$oldname" "$newname";
        git push origin -u "$newname";
}
function archive () {
        git checkout $1
        git checkout dev-cleanup
        git tag archive/$1 $1
        rm_local_branch $1
        rm_remote_branch $1
}

# <------ Programs -------> #
alias loadbash='source ~/.bash_profile'
alias loadzsh='source ~/.oh-my-zsh'
alias remove_condabase='conda config --set auto_activate_base false'
```

See [more useful examples](https://natelandau.com/my-mac-osx-bash_profile/)
of stuff to put in `~/.bash_profile`.
Also, see [this](https://joshstaiger.org/archives/2005/07/bash_profile_vs.html)
for a difference between `~/.bashrc` of Ubuntu and `~/.bash_profile` on MacOS.


## Setting up tools and programs

* `homebrew`
* `python`
* `chrome` (and/or other browsers)
* `git` and `github`
* `sublime`
* `vscode`
* `vim`
* `MS office`(incl. OneNote)
* Sync tools (Drive/Dropbox)
* `docker` and `dockerhub`
* `virtualenv`
* `conda`, `miniconda`, `miniforge`
* `tmux`
* `wandb`
* `slack`
* `notion`
* `typora`
* `AWS` (check student credits)
* `Google Collab`(check student credits)
* Useful commands at disposal
  * `tree`
  * `ncdu`
  * `rsync`
  * For generic programs, install with `brew`

See [this](https://levelup.gitconnected.com/im-programming-on-a-macbook-and-here-are-the-tools-that-make-my-life-easier-905b74b48c6d)
for a more extensive list.
Also, check out [this]().


Other useful resources:

1. [How To Setup A New MacBook Air M1 For Software Development](https://www.youtube.com/watch?v=gMsZT7yhz4Y)
2. [How to Setup a Brand new Macbook](https://www.freecodecamp.org/news/how-to-set-up-a-brand-new-macbook/)
