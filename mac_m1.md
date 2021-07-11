---
layout: default
title: Setup on a new Mac M1
nav_order: 4
---

# Setup on a new Mac M1
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

There has been a lot of buzz around the brand new Mac M1, and rightfully so. To me, the biggest boosts are the amazing compute and the battery life. However, there is some hesitation among the ML developer community since a lot of the most popular packages have not been build specifically for the new Apple Silicon chip. However, there are workarounds as pointed out by the 100s of blogs on the matter already.

In this section, I will try to describe a setup that is (a) easy to setup and (b) easy to maintain with a focus on PyTorch-based dependencies for ML development. Note that the key issue is that the new machine is based on `arm64` architecture while the Intel-based machines are based on `i386`. The setup below shall work with the native architecture as well as on `i386` architecture via Rosetta - which basically converts scripts runnable on `i386` to work on `arm64`.


## Setup two terminals

* Download [iTerm2](https://iterm2.com/).
* Make a copy and rename it “iTerm Rosetta”
* Select “Open with Rosetta” in “Get info”.
* In original terminal, check if the output of `arch` command is `arm64` and that in a Rosetta terminal is `1386`.

Basically, if you want to run something directly on the native chip, you shall use `iTerm2` and if you want to run something via Rosetta, use `iTerm2 Rosetta` terminal.

## Setup two package managers

The most common package managers include `conda`, `virtualenv`, etc. For this tutorial, I will rely on `conda` - the reason for which shall be clear by the end of this sub-section.

### Miniforge for `arm64`

Miniforge is built on Miniconda which itself is like a less bulky version of Anaconda. The amazing thing about miniforge is that their developers have already built binaries for many packages like `Pytorch` which shall run natively on M1 machines. Thus, we prefer managing dependencies with miniforge for running stuff natively.

* Open `iTerm`
* Download `miniforge3` (`Miniforge3-MacOSX-arm64.sh`) from the [official repo](https://github.com/conda-forge/miniforge) and run the following: (Use bash if you use a bash shell) 
    ```zsh
    mv ~/Downloads/Miniforge3-MacOSX-arm64.sh ~/.
    zsh Miniforge3-MacOSX-arm64.sh
    ```
* Follow the installation steps.

### Miniconda for `i386`

Similar to the previous subsection, now in `iTerm Rosetta`, download `Miniconda3-py39_4.9.2-MacOSX-x86_64.sh`  from [official docs](https://docs.conda.io/en/latest/miniconda.html) and follow similar steps. 

### Setting up base smartly

Now, you will realise that either you can have `miniforge` base or the `miniconda` base set by `conda init` in your `.zshrc` file (or `.bashrc`). But you can set the base smartly based on which terminal you are loading. Replace the `conda init` block by the following:

```zsh
ARCH=$(arch)

if [ "$ARCH" = "arm64" ]; then
    conda_manager="miniforge3";
else;
    conda_manager="miniconda3";
fi

conda_path="/Users/piyush/$conda_manager/bin/conda"

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!c
__conda_setup="$($conda_path 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/Users/piyush/$conda_manager/etc/profile.d/conda.sh" ]; then
        . "/Users/piyush/$conda_manager/etc/profile.d/conda.sh"
    else
        export PATH="/Users/piyush/$conda_manager/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

Depending on `arch`, we set the package manager to be `miniforge3` or `miniconda3`. That's the trick!

Now, if you open `iTerm` and run `python`, you should see:
```zsh
(base) ➜  ~ [3:00:59] $ python
Python 3.9.5 | packaged by conda-forge | (default, Jun 19 2021, 00:24:55)
[Clang 11.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

If you open `iTerm Rosetta` and run `python`, you should see:

```zsh
(base) ➜  ~ [3:04:24] $ python
Python 3.9.1 (default, Dec 11 2020, 06:28:49)
[Clang 10.0.0 ] :: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```


## Installing PyTorch et al

### ARM64

1. Open `iTerm2`
2. Create & activate conda environment
   ```zsh
   conda create -n core-arm64-py3.9
   conda activate core-arm64-py3.9
   ```
3. Install `python3.9`
   ```zsh
   conda install python=3.9
   ```
4. Install key packages (Note that this could have been done using `conda` but the wheel files from `pip` are ready as of July 11, 2021 - so why not use it!)
   ```zsh
   pip install torch torchvision torchaudio torchtext
   ```
   Check the packages by importing them in a python shell.
5. Install other packages:
   1. `conda install scikit-learn` (no pip alternative works)
   2. Follow [this](https://sayak.dev/install-opencv-m1/) to install `opencv-python` from source. (no other alternative works)

> **Note**: `torchaudio` import works fine but with the following warning - I was not able to fix this by installing `PySoundFile`/`sox`.
> ```python
> >>> import torchaudio
> /Users/piyush/miniforge3/envs/core-arm64-py3.9/lib/python3.9/site-packages/torchaudio/backend/utils.py:67: > UserWarning: No audio backend is available. warnings.warn('No audio backend is available.')
> ```

### i386

1. Open `iTerm2 Rosetta`
2. Create & activate conda environment
   ```zsh
   conda create -n core-i386-py3.9
   conda activate core-i386-py3.9
   ```
3. Install `python3.9`
   ```zsh
   conda install python=3.9
   ```
4. Install key packages (Note that this could have been done using `conda` but the wheel files from `pip` are ready as of July 11, 2021 - so why not use it!)
   ```zsh
   pip install torch torchvision torchaudio torchtext -c pytorch
   pip install scikit-learn
   pip install opencv-python
   pip install jupyter jupyterlab
   ```
   Note that, here, we do not have to worry about something not being installed by `pip` since all the whl files are already there while working with `1386`.


## Addendum

You can also install two different `brew`'s depending on which arch you want to use. [This](https://soffes.blog/homebrew-on-apple-silicon) is a nice tutorial you can follow. I would recommend using `brew` with the native arch by default and having an alias like `ibrew` for the latter.

## References

1. [How to Install Rosetta 2 on Apple Silicon Macs](https://osxdaily.com/2020/12/04/how-install-rosetta-2-apple-silicon-mac/)
2. [Using Conda on an M1 Mac](https://towardsdatascience.com/using-conda-on-an-m1-mac-b2df5608a141) by [Nils Flaschel](https://pooled-roc-xgb-plotly.medium.com/)
3. [Building and Installing opencv 4.5.0 on Mac M1](https://sayak.dev/install-opencv-m1/) by [Sayak Paul](https://sayak.dev/)
4. [Homebew on Apple Silicon](https://soffes.blog/homebrew-on-apple-silicon) by [Sam Soffes](https://soff.es/)

If you have suggestions or if something did not work for you, please feel free to [email](mailto:piyushnbagad11@gmail.com) me or DM on [Twitter](https://twitter.com/bagad_piyush?lang=en).
