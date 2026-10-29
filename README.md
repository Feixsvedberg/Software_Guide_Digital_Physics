# Software Beginners Guide for Digital Physics

**Author:** Felix Svedberg F22  
**Date:** October 2026

## Table of contents

- [Background](#background)
- [Python](#python)
  - [Installing Python](#installing-python)
    - [Windows](#windows)
    - [Mac](#mac)
    - [Linux](#linux)
  - [Virtual Environments (venv)](#virtual-environments-venv)
  - [Creating and activating a venv](#creating-and-activating-a-venv)
    - [Windows](#windows-1)
    - [Mac](#mac-1)
    - [Linux](#linux-1)
  - [Installing packages to the venv](#installing-packages-to-the-venv)
  - [Notebooks](#notebooks)
    - [Jupyter Notebook](#jupyter-notebook)
  - [Python as a language](#python-as-a-language)
  - [Alternative to venv - uv](#alternative-to-venv---uv)
- [Git](#git)
  - [GitHub](#github)
  - [Git commands](#git-commands)
- [AJX](#ajx)
  - [Running an AJX script](#running-an-ajx-script)
  - [AJX as a package](#ajx-as-a-package)
  - [Summary](#summary)
- [AGX](#agx)
  - [AGX installation](#agx-installation)
  - [Running an AGX tutorial](#running-an-agx-tutorial)
  - [Running your own AGX Python script](#running-your-own-agx-python-script)
- [Troubleshooting and FAQ](#troubleshooting-and-FAQ)

---

## Background

In the course Digital Physics, you will mainly be working with two software tools: **AGX**, a software development kit for modeling and simulation of mechanical systems with contacts and friction, and **AJX**, a minimal multi-body dynamics simulator written in Python.

Most students of Teknisk Fysik at Umeå University are familiar with C and Matlab, but not everyone has experience with Python. This manual therefore covers the Python basics you need for the course. It also introduces Git and GitHub and explains how to get started with AJX and AGX.

## Python

Python is a high-level, general-purpose programming language and is generally regarded as the most well-known programming language in the world. Python is also the language you will use during this course! Many of you are probably familiar with Python, but for those who are not, I have written this section, which will go through basics such as downloading Python, Virtual Environments and Notebooks.

There already exists a good tutorial for the Python basics written by S. Fatemi, J. Siehr and D. Waller for the course Machine Learning in Physics: [PythonJumpstart](https://gitlab.exphys.umu.se/mlp/pythonjumpstart). Much of the material in this section is taken from this repository, but only parts relevant to Digital Physics are included. This manual also includes some additional explanations of the most important concepts.

### Installing Python

In this section, we explore how to install Python on different operating systems. Installing Python means installing a Python interpreter (which interprets and executes your Python code), some supporting tools, and the standard library.

#### Windows

Installing Python on Windows:

- Visit [Python Downloads](https://www.python.org/downloads) and download the desired version of Python.
- Run the installer.
- **Very important:** on the first installer screen, check the box named **Add python.exe to PATH**. Adding Python to PATH means Windows will know where Python is installed. Without this, commands like `python` and `pip` may not work in PowerShell.
- Click **Install Now**.

If you already installed Python and forgot to check Add python.exe to PATH, do this:

- Run the Python installer again.
- Click **Modify**.
- Click **Next**.
- Check **Add Python to environment variables**.
- Click **Install** or **Modify** to finish.

If you cannot find the installer anymore, download it again from [Python Downloads](https://www.python.org/downloads) and run it.

After installation, open PowerShell and check:

```powershell
python --version
pip --version
```

If `python` does not work, try:

```powershell
py --version
```

If `py --version` works but `python --version` does not, Python is installed, but it was probably not added to PATH. Follow the steps above to modify the Python installation and add it to environment variables.

`pip` is Python's package installer and will be important later.

#### Mac

In order to install Python for MacOS, follow these steps:

- Download desired Python version (**TODO: Which version should I recommend? 3.12?**) from [Python Downloads for macOS](https://www.python.org/downloads/macos/).
- If you already have it, you can use Homebrew: `brew install python`.

Check if it's installed by running the following in the Terminal:

```bash
python3 --version
pip3 --version
```

`pip3` is Python's package installer and will be important later.

#### Linux

On Ubuntu or Debian:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv
```

On Fedora:

```bash
sudo dnf install python3 python3-pip
```

On Arch Linux:

```bash
sudo pacman -S python python-pip
```

Check if installed:

```bash
python3 --version
pip3 --version
```

`pip3` is Python's package installer and will be important later.

### Virtual Environments (venv)

A virtual environment (or venv) is a crucial tool when building and testing Python projects. But what is it? A venv is an isolated environment on your computer in which you control what version of the Python interpreter and what packages and what package versions you want to use in your project. Think of them as containers of a certain set of Python tools that your specific project needs. venvs are isolated from other venvs, so you can use different venvs for different projects.

Consider this example:

- **Project A:** Uses Python version 3.12 as the interpreter and Numpy 3.3 as a package.
- **Project B:** Uses Python version 3.14 as the interpreter and Numpy 3.5 as a package.

In this case, it is suitable to create two different venvs: `venvA` and `venvB`. When I then want to work with Project A, I will simply activate `venvA`.

### Creating and activating a venv

In the following section, I walk you through how to create and activate a venv for different operating systems. One thing that is worth clarifying is that the following commands are to be run **inside your project folder**.

#### Windows

To **create** a venv, use this command in PowerShell:

```powershell
python -m venv venv_name
```

To **activate** `venv_name`, use:

```powershell
.\venv_name\Scripts\Activate.ps1
```

If this worked, your terminal line should start with `(venv_name)`.

If PowerShell blocks the activation, run the following command once:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then try to activate again.

#### Mac

To **create** a venv, run this command in the Terminal:

```bash
python3 -m venv venv_name
```

To activate `venv_name`, use:

```bash
source venv_name/bin/activate
```

If this worked, your terminal line should start with `(venv_name)`.

#### Linux

In order to create and activate a venv on Linux, use the same commands as on Mac.

### Installing packages to the venv

To install a package to your venv, we will use pip, which was mentioned earlier. Say that you want to install version 2.5.1 of NumPy, a very common package used for numerical computations; you would first make sure that your venv is activated and then run the following commands:

```bash
python -m pip install --upgrade pip
python -m pip install "numpy =>2.5.1"
```

The first command updates your current version of pip, and the second one installs NumPy to your venv.

In many cases, one is interested in installing a bunch of packages at once in order to save time. One could just write a long command with the desired packages as:

```bash
python -m pip install numpy scipy matplotlib
```

But there are more efficient ways of doing this if there are many required packages.

A common scenario is wanting to use a project written by someone else with a bunch of required packages. It is then common that these packages are listed in a text file in the project (often named `requirements.txt` or `dependencies.txt`). If you want to install all of these packages, you can use:

```bash
python -m pip install -r "requirements.txt"
```
It is worth mentioning that when using pip, the command

```bash
pip install
```
instead of 
```bash
python -m pip install
```
is often sufficient. It is however, safer to use the latter alternative since it helps ensure that packages are installed using the intended Python interpreter. This is especially useful when multiple Python versions or virtual environments are present.

### Notebooks

Using notebooks, which you will encounter during the course, is a simple and intuitive way of working with Python. A notebook combines executable code, code output, explanatory text and graphic visualization all in one file (with suffix `.ipynb`). The notebook contains cells, which are executed one at a time. The cells are either code or Markdown cells.

There are multiple ways of working with notebooks:

- Open the project folder in Visual Studio Code.
- Open the project folder in PyCharm.
- Use Jupyter Notebook in your browser.

In this manual, I will explain how to do it using Jupyter Notebook, but if you are interested in the other methods you can read about them in [PythonJumpstart](https://gitlab.exphys.umu.se/mlp/pythonjumpstart).

#### Jupyter Notebook

Jupyter Notebook is a Python package and it is downloaded to your venv by running:

```bash
python -m pip install notebook
```

You can then start a local Jupyter Notebook server by simply running:

```bash
jupyter notebook
```

Your browser should open automatically. If it does not, copy the URL shown in PowerShell and paste it into your browser.

### Python as a language

How to write Python code will not be covered in this manual, but I once again strongly recommend that you visit [PythonJumpstart](https://gitlab.exphys.umu.se/mlp/pythonjumpstart) where you can find multiple notebooks with tutorials for beginners!

### Alternative to venv - uv

There is a more efficient alternative to venv called `uv`. It is a fast Python project and package manager that combines functionality from several tools, including Python version management, venv for creating virtual environments, and pip for installing packages. Using uv is optional, but those who are interested can find a complete guide for uv here: [uv - Complete Guide](https://pydevtools.com/handbook/explanation/uv-complete-guide/).

Another big advantage of using uv instead of venv is that uv maintains a shared cache (storage where data is saved for reuse) of Python packages. When multiple project environments need the same package, uv can reuse cached files instead of installing the package again. Depending on the operating system and filesystem, it can also reduce disk usage by avoiding duplicate copies of package files. This can save both time and disk space!

## Git

Git is a free and open-source version control system (VCS). It is by far the most widely used VCS globally, and I can almost guarantee that being familiar with Git is going to be very useful in your career as an engineer!

I will not go into detail of every functionality of Git, but I will provide you with links to relevant material and highlight the most important concepts. A good Git intro can be found here: [W3Schools - Git & GitHub](https://www.w3schools.com/git/git_intro.asp?remote=github).

The first thing you need to do is install Git. How to do this is found on [W3Schools - Git & GitHub](https://www.w3schools.com/git/git_intro.asp?remote=github) under **Git install** and **Git Config**.

### GitHub

GitHub is an online platform for storing, sharing, and managing code. It uses Git to keep track of changes to a project and makes it easier for developers to collaborate. You can think of Git as the tool that tracks changes to your files, while GitHub is a place where you can store your project online and share it with others. GitHub is relevant to this course since AJX is hosted there.

You will need to create a GitHub account. Follow the steps under **GitHub Get Started**, **Git What is SSH** and **GitHub add SSH** in [W3Schools - Git & GitHub](https://www.w3schools.com/git/git_intro.asp?remote=github).

### Git commands

There are a bunch of commands that are useful when working with Git. The most important ones are found here: [Git Cheat Sheet](https://git-scm.com/cheat-sheet). For this course, I advise that you get familiar with at least `git clone` (cloning an existing repository, e.g. AJX) and `git pull` (fetch changes from the remote repository and merge them with your local clone). You will use the latter if, for example, notebooks in AJX have been updated.

## AJX

AJX is a minimal multibody dynamics project written in Python. It is hosted on GitHub, and can be accessed through the following link: [AJX repository](https://github.com/Hman05/AJX).

When accessing the link, you will enter GitHub's GUI. The first thing you see is the repository structure itself, which is just like any folder. Displayed under the repository structure is a readme. This file contains information about the project, what Python packages are required to run the files, the commands for running the scripts, and also the command used to install AJX.

### Running an AJX script

> **Note:** The instructions in this subsection are only for running example scripts. You will also be required to install AJX for future coursework. How to install it is detailed in [AJX as a package](#ajx-as-a-package).

I will now list the steps required to get an AJX example script running!

1. Create a folder for AJX (I would not name it AJX, though, since that is the name of the repository which you will clone in the next steps).
2. Create a venv in this folder (see [Creating and activating a venv](#creating-and-activating-a-venv)).
3. Activate the venv.
4. Download the Python packages listed in the readme.
5. Clone the repository to the folder by running the following command:

   ```bash
   git clone https://github.com/Hman05/AJX
   ```
   or if you are using SSH (recommended):
   ```bash
   git clone git@github.com:Hman05/AJX.git
   ```

   If this was successful, you should now see a repository called `AJX` inside your folder. If you are unsure what any of this means or if you do not have Git installed, visit the [Git](#git) section.
7. Navigate to the project root (the "highest level" of the repository).
8. Run the following command:

   ```bash
   python -m scenes.cartpole
   ```

   If all packages were downloaded correctly and the repository cloned successfully, you should now see a cart with an attached pole sliding along a rail!

### AJX as a package

You should also install AJX as a Python package by navigating to the project root of the AJX repository, which you just cloned to your folder, and running the following command:

```bash
python -m pip install -e .
```

By downloading it as a package, you will be able to import modules from AJX to different programs anywhere in your venv.

### Summary

To clarify the instructions written above:

Cloning is needed to access the full repository. This means all the directories listed in the [AJX repository](https://github.com/Hman05/AJX). Many of these directories contain files that will be used in future coursework, so having access and the ability to get the latest versions via `git pull` is going to be important. Read the [Git](#git) section if you're not familiar with `git pull`.

Installing it as a package is needed to use AJX in your own programs anywhere in the venv. You should do both to efficiently work with AJX during the course.

## AGX

AGX is a software development kit for modeling and simulation of mechanical systems with contacts and friction. It can be used for analysis and design, or as a “physics engine” to drive interactive applications for training, in 3D of course, or as a virtual training ground for AI and ML. It is developed by the Umeå-based company Algoryx.

### AGX installation

To get AGX installed, follow these steps:

1. Visit [AGX download](https://www.algoryx.se/download/) and choose the latest version for your OS. After the download is complete, start the installation executable. This should open a window asking if you wish to continue with installation.
2. Visit the [AGX Installation Guide](https://www.algoryx.se/documentation/complete/agx/tags/agx-2.34.0.0/UserManual/source/installation.html) and follow the steps for your OS to finish the installation. Make sure to do the following:
   - Check the box **Install python libraries?**
   - Uncheck the box **Run the license authorization program?**
3. Place the license file found on Canvas (**TODO: Insert link**) in the installation folder, i.e. the folder called `AGX-2.xx` (where `xx` is your installed version).

AGX should now be installed!

### Running an AGX tutorial

I will now go through the steps to get an AGX simulation running. First, we need to set up the environment. Open the Command Prompt/Terminal and navigate to the installation folder. Now, use the command:

- Windows:

  ```bat
  call setup_env.bat
  ```

- Linux or Mac:

  ```bash
  source setup_env.bash
  ```

to set up the environment. Remember what you have learned about activating venvs before running Python programs; this is similar! It is however, not exactly the same. `setup_env.bat` configures environment variables and Python interpreter. For example, it sets paths that allow Python to find packages included with the AGX installation.

By default, AGX uses its bundled Python interpreter, which comes with packages already installed. However, you can tell AGX to use an external Python installation. This is controlled by the AGX_USE_EXTERNAL_PYTHON variable or by using the argument /USEEXTERNALPYTHON when calling `setup_env.bat`.

Once you are done with setting up the environment, you can run the following command:

```bash
agxViewer cables.agxPy
```

This opens and runs the simulation in the Python script `cables.agxPy`! If everything works correctly, you should now see a simulation of a couple of spheres connected by cables. `agxViewer` starts by default in paused mode. Remember to press `e` to start simulation.

Press `1` to reload the scenes.

`agxViewer` is an application for reading various files (`.agx`, `.aagx`, `.agxScene`, …). It is the default program for opening `.agx`, `.aagx`, `.agxPy`, and `.agxPyz` files.

### Running your own AGX Python script

We are now going to write our own AGX simulation by modifying an existing example! Do the following:

1. Create a folder for your own AGX Python scripts anywhere on your computer.
2. Visit [AGX Python tutorials](https://www.algoryx.se/documentation/complete/agx/tags/latest/doc/main/python_tutorials.html), copy the source code for the Basic simulation tutorial, create a Python file named `basic_sim.py`, and paste the source code into it.
3. Comment out line 19, `from tutorial_utils import createHelpText`, and line 23, `createHelpText(sim, app)` (`tutorial_utils` is not defined in the AGX download). **TODO: Is there a better AGX example to start from?**
4. Make sure the AGX environment is set up in your terminal/command prompt before trying to start the simulation. See [Running an AGX tutorial](#running-an-agx-tutorial) to learn how to do this.  
5. Start the simulation:

   ```bash
   agxViewer basic_sim.py
   ```

   You should now see a simulation of a sphere dropped onto a surface.
6. In this final step, I will actually give a very simple exercise! I want you to change this sphere into a box. Go to [AGX Documentation](https://www.algoryx.se/documentation/complete/agx/tags/latest/doc/html/index.html) and look around to figure out how! (Documentation is, however, written for C++, but the syntax for Python is very similar.)

---

## Troubleshooting and FAQ


This is it for this manual. Don't hesitate to contact the course supervisors if you are experiencing problems with any of the steps! They are very helpful and usually respond very fast!

I wish you all the best of luck with future coursework! 
