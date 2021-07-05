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

### Conda basics

* Create a new enviornment with python version
  ```bash
  conda create -n myenv python=3.8
  ```
* List all environments
  ```bash
  conda env list
  ```
* List all packages and versions installed in active environment
  ```bash
  conda list
  ```
* Delete an environment and everything in it
  ```bash
  conda env remove --name myenv
  ```
* Use conda to search for a package
  ```bash
  conda search PACKAGENAME
  ```
See [this cheatsheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf) for more useful commands.

Typically, I install packages within `conda` env with `pip install PACKAGENAME`. To (concisely) list all packages in a conda environment installed via pip, use:
```bash
pip install pip-chill
pip-chill -v > concise_requirements.txt
```

### Docker basics

* Download and install `docker` following [this](https://docs.docker.com/get-started/#download-and-install-docker)
* Creating a new image via `Dockerfile`: follow [this tutorial](https://stackify.com/docker-build-a-beginners-guide-to-building-docker-images/), see [this](https://github.com/WadhwaniAI/cough-against-covid) for a reference.
  * To build a docker image from Dockerfile:
    ```bash
    cd /location/of/your/Dockerfile
    docker build -t yourusername/repository-name .
    ```
  * To commit and push to dockerhub.com (see more on [commit](https://docs.docker.com/engine/reference/commandline/commit/) and [push](https://docs.docker.com/engine/reference/commandline/push/))
    ```bash
    docker commit -m "message" CONTAINER [REPOSITORY[:TAG]]
    docker push [REPOSITORY[:TAG]]
    ```

## Coding workflow

### Editor
I prefer to code in `vscode` setup directly on the remote machine. Some useful packages to install within VSCode:
```
Docker
Jupyter
PyLance
PyTorch Snippets
Markdown - All in One
reStructuredText
3D Viewer for VSCode
Python Docstring Generator
```
and lots more.

### Unit testing
I use `unittest` framework for testing - see [this](https://realpython.com/python-testing/#unittest) for a quick overview.

**Code standardization** (linting):

- [Intro tutorial](https://docs.pylint.org/en/1.6.0/tutorial.html)
- [List of message IDs with their meanings](https://docs.pylint.org/en/1.6.0/features.html)
- [FAQ for Pylint](http://pylint.pycqa.org/en/latest/faq.html)
- [Adding Pylint checks as pre-commit hooks](https://git-pylint-commit-hook.readthedocs.io/en/latest/usage.html#pylint-configuration) (so that you cannot commit if your Pylint score is < some threshold)
- [How to disable certain Pylint checks](https://docs.pylint.org/en/1.6.0/faq.html#message-control)

### Documentation
I use `Python Docstring Generator` extension on VSCode to generate Google format docstrings. For usual documentation (e.g. installation), I use READMEs inside individual folders.

### Sphinx for code docs 

- Suppose your repo resides at `/path/to/your/repo/`
  ```bash
  cd /path/to/your/repo/
  mkdir doc/
  cd doc/
  ```
- Install `sphinx` using `pip install sphinx`
- Run the following command and fill in the details `sphinx-quickstart`
- Adding a custom theme:
  - Install a new theme
    ```bash
    pip install rtcat_sphinx_theme
    pip install recommonmark
    ```
- Add following lines in `doc/conf.py`
  ```bash
  html_theme = 'rtcat_sphinx_theme'
  import rtcat_sphinx_theme
  html_theme_path = [rtcat_sphinx_theme.get_html_theme_path()]
  ```

- Adding API Reference for code:
  - Edit conf.py as follows: 
    ```bash
    exclude_patterns = ['_build', 'Thumbs.db', '.DS_Store', '_api']
    ```
  - Make a folder named _api inside doc

**Resources**

- [Sphinx tutorial](https://buildmedia.readthedocs.org/media/pdf/brandons-sphinx-tutorial/latest/brandons-sphinx-tutorial.pdf)
- [Code for sphinx tutorial](https://github.com/brandon-rhodes/sphinx-tutorial/tree/master/triangle-project/trianglelib)
- [Sphinx documentation](https://www.sphinx-doc.org/en/master/contents.html)
- [Example sphinx documentation file from sphinx's main repo](https://github.com/sphinx-doc/sphinx/blob/3.x/doc/contents.rst)

> (Some of these sphinx instructions have been written by Aman Dalmia)


### Pre-commit hooks
TODO

### Other basic software practices/tools

- **Scripting**: Sublime and shortcuts, Sublime SFTP, jupyter lab and shortcuts, basic bash scripting, tmux and tmux shortcuts, mosh, forticlient, linux aliases, autoreload in jupyter, regex (python)
- **Generic**: general MAC shortcuts, ssh, rsync, ssh-keys, htop and itop, nvidia-smi, gpustat
- **Debugging**: ipdb
- **Deep learning frameworks**: Pytorch, TF2.0, fastai (optional)


**Useful links**:
* [PyTorch support in VS Code](https://code.visualstudio.com/docs/datascience/pytorch-support)
* [Missing Semester of your CS Education](https://missing.csail.mit.edu/)


## ML Codebase Design

TODO