---
layout: default
title: Efficient Workflow
nav_order: 4
---

# Efficient Workflow
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

In this section, I note down all the things that make my code workflow easier, convinient and efficient in terms of time and effort. This is centered around ML development.


## Git and GitHub

### Aliases and functions

This has already been described in the [Getting started on a new machine](new_machine.md#Setting-up-bash-aliases-&-others) section.

### Setting up SSH keys for multiple Github accounts

Follow [this tutorial](https://gist.github.com/oanhnn/80a89405ab9023894df7). This is especially useful when you have a personal account and a work account.

### Setting up SSH keys on a new machine

```bash
git config --global user.name "Piyush (Machine XYZ)"
git config --global user.email "piyush@xyz.com"

eval `ssh-agent`
ssh-add -l

ssh-keygen -t rsa -b 4096 -C "piyush@xyz.com"
cat ~/.ssh/id_rsa.pub
```

Copy the output. Login to GitHub with the same email ID. Then go to [https://github.com/settings/ssh/new](https://github.com/settings/ssh/new) add a Title and paste the copied output and press `Add SSH key`.