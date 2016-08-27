---
layout: post
title:  "Effective python implementation"
date:   2015-11-05
excerpt: "A brief introduction to the python ecosystem"
tag:
- python 
- pip
- virtualenv
- virtualenvwrapper
comments: true
---

## Version perplexion

Python 3 is a different language from Python 2. There are some subtle and some stark semantic and syntactic differences. As of today, Python 2.x is the most installed and most used version. Many mainstream and important packages/frameworks/tools/utilities/modules are not yet 100% compatible with Python 3.

But, Python 3 is the future and is getting as well supported as Python 2 was in terms of libraries, documentation, online references and so on. Python 2 is the past, so it is going to be progressively discontinued and is already obsolete in a sense.

---

## Choice of VM

The Python interpreter or the Python Virtual Machine has a number of different implementations, CPython being the main and most popularly installed implementation. CPython also acts as the reference implementation for other virtual machines.

PyPy is Python implemented in Python, Jython is implemented in Java and runs on the Java VM and IronPython is the Python implementation for Microsoft .NET CLR.

---

## Installing python on OS X El Capitan

While Python comes pre-installed on OS X it is not recommended to use this binary for development. 

***Rules of thumb:*** 
1. Never use the pre-installed OS X Python for development. 
2. Use Python with Homebrew. 
3. Never ever sudo pip(with this setup,ie brewed python, you’ll never have to use sudo to install any Python relative stuff. If you end up doing this, something is broken in your brewed Python installation)
4. Use virtualenv / virtualenvwrapper. 

***Steps:***
1. Install homebrew
2. Install python2 using homebrew
3. Installing virtualenv using pip globally
4. Upgrade pip to latest version
5. Install python3 using homebrew
6. Install pythonenvwrapper globally using pip
7. Create .bashrc(shell startup file) file and add 'source /usr/local/bin/virtualenvwrapper.sh'
8. Run/refresh the .bashrc file
9. Create a virtualenv for python2
10. Create a virtualenv for python3

---

Lets briefy discuss the tools we are going to install before proceeding with the steps of installation. 

## homebrew
Homebrew is a package manager designed for installing UNIX tools and other open-source applications on Mac OS X. It will quickly download and install them, compiling them from source. 

## pip
pip is a package manager. It is a tool for installing Python packages from the Python Package Index. PyPI (which you'll occasionally see referred to as The Cheeseshop) is a repository for open-source third-party Python packages. It's similar to bundler in the Ruby world or NPM in the node world.
Python actually has another, more primitive, package manager called easy_install, which is installed automatically when we install Python itself. pip is vastly superior to easy_install for lots of reasons, and so should generally be used instead.  

---

## virtualenv

virtualenv is a tool to keep the dependencies required by different projects in separate places, by creating virtual Python environments for them. It solves the “Project X depends on version 1.x but, Project Y needs 4.x” dilemma, and keeps the global site-packages directory clean and manageable.

To illustrate this, let's start by pretending virtualenv doesn't exist. Imagine we're going to write a Python program that needs to make HTTP requests to a remote web server. We're going to use the Requests library, which is brilliant for that sort of thing. As we saw above, we can use pip to install Requests.
But where on our computer does pip install the packages to? Here's what happens if we try to run pip install requests:
```
$ pip install requests
Downloading/unpacking requests
  Downloading requests-1.1.0.tar.gz (337Kb): 337Kb downloaded
  Running setup.py egg_info for package requests

Installing collected packages: requests
  Running setup.py install for requests
```
So, we now know that we can import requests and use it in our program. The software works brilliantly, we make loads of money, and our clients are so impressed that they ask us to write another program to do something slightly different.
But this time, we find a brand new feature that's been added to requests since we wrote our first program that we really need to use in our second program. So we decide to upgrade the requests library to get the new feature:
pip install --upgrade requests

Everything seems fine, but we've unknowingly created a disaster!
Next time we try to run it, we discover that our original program has completely stopped working and is raising errors when we try to run it. Why? Because something in the API of the requests library has changed between the previous version and the one we just upgraded to. It might only be a small change, but it means our code no longer uses the library correctly. Everything is broken!

Sure, we could fix the code in our first program to use the new version of the requests API, but that takes time and distracts us from our new project. And, of course, a seasoned Python programmer won't just have two projects but dozens - and each project might have dozens of dependencies! Keeping them all up-to-date and working with the same versions of every library would be a complete nightmare.

How does virtualenv help?
virtualenv solves this problem by creating a completely isolated virtual environment for each of our programs. An environment is simply a directory that contains a complete copy of everything needed to run a Python program, including a copy of the python binary itself, a copy of the entire Python standard library, a copy of the pip installer, and (crucially) a copy of the site-packages directory mentioned above. When we install a package from PyPI using the copy of pip that's created by the virtualenv tool, it will install the package into the site-packages directory inside the virtualenv directory. We can then use it in your program just as before.

### Basic Usage:
Create a virtual environment for a project:

```
$ cd my_project_folder
$ virtualenv venv
```

virtualenv venv will create a folder in the current directory which will contain the Python executable files, and a copy of the pip library which you can use to install other packages. The name of the virtual environment (in this case, it was venv) can be anything; omitting the name will place the files in the current directory instead.

This creates a copy of Python in whichever directory you ran the command in, placing it in a folder named venv.

We can also use a Python interpreter of our choice.
`$ virtualenv -p /usr/bin/python2.7 venv`
This will use the Python interpreter in /usr/bin/python2.7

To begin using the virtual environment, it needs to be activated:
`$ source venv/bin/activate`
The name of the current virtual environment will now appear on the left of the prompt (e.g. (venv)Your-Computer:your_project UserName$) to let you know that it’s active. From now on, any package that we install using pip will be placed in the venv folder, isolated from the global Python installation.

Install packages as usual, for example:

```
$ pip install requests
```

If we are done working in the virtual environment for the moment, we can deactivate it:

```
$ deactivate
```

This puts us back to the system’s default Python interpreter with all its installed libraries. To delete a virtual environment, just delete its folder.(In this case, it would be rm -rf venv).

The virtualenv folder we created can reside anywhere(can even reside inside the codebase). In that case we need to remember to exclude the virtual environment folder from source control by adding it to the ignore list, ie if we using git then we need to add this virtualenv folder to the .gitignore file.

#### Other options:

Running virtualenv with the option --no-site-packages will not include the packages that are installed globally. This can be useful for keeping the package list clean in case it needs to be accessed later. [This is the default behavior for virtualenv 1.7 and later.]

In order to keep our environment consistent, it’s a good idea to “freeze” the current state of the environment packages. To do this, run

```
$ pip freeze > requirements.txt
```

This will create a requirements.txt file, which contains a simple list of all the packages in the current environment, and their respective versions. We can see the list of installed packages without the requirements format using “pip list”. Later it will be easier for a different developer (or us, if we need to re-create the environment) to install the same packages using the same versions:

```
$ pip install -r requirements.txt
```

This can help ensure consistency across installations, across deployments, and across developers.

---

## virtualenvwrapper

Working with multiple virtualenvs can become really cumbersome. We have to switch and rememberthe location of the virtualenv we have created. This is where virtualenvwrapper comes to place: virtualenvwrapper is a set of extensions to virtualenv tool. We can easily list, create and delete virtual environments; all of our virtual environments are organized in one place.

We can create a new virtualenv easily:
```
$ mkvirtualenv env1
New python executable in env1/bin/python2.7
Also creating executable in env1/bin/python
Installing setuptools, pip…done.
(env1)~ $
By default, all your virtual environments will be created under ~/.virtualenvs/ but you can change it.
```

List all virtual environnements:
```
$ workon
env1
env2
```
Switch to a virtualenv:
```
$ workon env1
```
Deactivating is still the same:
```
$ deactivate
```
Delete a virtualenv:
```
$ rmvirtualenv venv
```

### Other useful commands
`lsvirtualenv`
List all of the environments.

`cdvirtualenv`
Navigate into the directory of the currently activated virtual environment, so we can browse its site-packages, for example.

`cdsitepackages`
Like the above, but directly into site-packages directory.

`lssitepackages`
Shows contents of site-packages directory.

---

## Installation

***Step1:***
Lets install homebrew. Run the following command in the terminal.

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

***Step2:***
Lets install python2 using homebrew

```
$ brew install python
```

This will install python2 and pip(package manager). It actually installs the binary in /usr/local/Cellar/python/2.7.11/bin/python(may vary based on the latests stable version of python). /usr/local/bin/python is a symlink.

which python
/usr/local/bin/python

which pip
/usr/local/bin/pip

***Step3:***
Lets instally virtualenv using pip globally.

```
$ pip install virtualenv
```
It installs virtualenv and prompts us to upgrade pip if its not the latest stable version. You can skip the next step if you already have the latest version of pip.

***Step4:***
Lets upgrade to the latest version of pip.

```
$ pip install --upgrade pip
```

***Step5:***
Lets install python3 using homebrew

```
$ brew install python3
```

This will install python3 and pip3(package manager). It actually installs the binary in /usr/local/Cellar/python/3.5.1/bin/python3(may vary based on the latests stable version of python). /usr/local/bin/python3 is a symlink.

which python
/usr/local/bin/python3

which pip
/usr/local/bin/pip3

***Step6:***
Lets install pythonenvwrapper globally using pip

```
$ pip install virtualenvwrapper
```
It will download and install virtualenvwrapper.

***Step7:***
Lets create .bashrc(shell startup file) file and add 'source /usr/local/bin/virtualenvwrapper.sh'

```
$ touch ~/.bashrc
```
Add source /usr/local/bin/virtualenvwrapper.sh to the end of the file

***Step9:***
Lets refresh the .bashrc file. Close the terminal and open it again 
OR

```
$ source ~/.bashrc
```

Automatically a dir named .virtualenvs/ will be created in the home directory. This is where all the virtual envs will be created when we use the mkvirtualenv command.

***Step9:***
Lets create a virtualenv for python2.

```
$ mkvirtualenv py2
```
```
$ which python
/Users/username/.virtualenvs/py2/bin/python
$ which pip
/Users/username/.virtualenvs/py2/bin/pip
$ python —version
Python 2.7.11
$ pip —version
pip 8.1.2 from /Users/username/.virtualenvs/py2/lib/python2.7/site-packages (python 2.7)
It creates the virtual env and automatically switches to that virtualenv.
```

```
$ deactivate
```
This brings it out of the py2 virtualenv.

***Step10:***
Lets create a virtualenv for python3.

```
$ mkvirtualenv -p /usr/local/bin/python3 py3
```
```
$ which python
/Users/username/.virtualenvs/py3/bin/python
$ which pip
/Users/username/.virtualenvs/py3/bin/pip
$ python --version
Python 3.5.1
$ pip --version
pip 8.1.2 from /Users/username/.virtualenvs/py3/lib/python3.5/site-packages (python 3.5)
```



