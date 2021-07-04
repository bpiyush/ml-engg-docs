---
layout: default
title: Efficient Workflow
nav_order: 3
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


## Coding environments

Investing some time upfront to set robust code environments is very useful in the longer run for that project. The most common code environments are: `virtualenv`, `conda` (`anaconda`, `miniconda`, `conda-forge`, `miniforge`) and `docker`.

* If you are extending someone else's code, or just trying out someone's code on your dataset, it is better to simply use their environment setup.
* For your own code: the fight is typically between `conda` and `docker`.
  * If it is a single isolated project, you may choose `conda`.
  * If it a project that is only going to require a few basic dependencies, then you may choose `conda`.
  * If it is a core project that you are going to have follow up projects on, I would recommend `docker`.
  * In cases where large teams are involved, again `docker` is the way to go.
  * In cases where deployement is likely to occur, again, choose `docker`.
  * It might also be useful to check what the lab uses prominently.

> *Docker* can be very useful if you already created images with specific dependencies, e.g. a `core` image with all vision, language, 3D etc dependencies, a `core-vision` image and so on.