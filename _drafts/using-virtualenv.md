---
tags: [tutorials, python]
---
Whether you use pip or easy_install to install Python packages, odds are you have
run into situations where the different projects you work on require different
and incompatible versions of packages. Perhaps you created your
personal website over 2 years ago on Django 1.6.5, and now you want to also
use Django for a school project. Rather than (1) use an outdated version of
Django for your new project or (2) upgrade your personal website to a new version
of Django every time you start a new Django project, you can instead use virtualenv.

# What is Virtualenv?
Virtualenv is a Python package that gives you a different Python environment for
each of your projects. Each of these have its own independent set of installed packages,
so you never have to run into package conflicts again.

# How Do I Use Virtualenv?
First, you have to install virtualenv.

```console
$ pip install virtualenv
```
You are now ready to use virtualenv. There are two main approaches.

## Approach 1: Using Virtualenvwrapper (Recommended)
The first approach is to install an additional package called virtualenvwrapper.
This is a set of command-line tools that makes virtualenv a little easier to use.

```console
$ pip install virtualenvwrapper
```
Virtualenvwrapper will also install a script `virtualenvwrapper.sh` to one of
your bin directories (on a Mac, this is at `/usr/local/bin/virtualenvwrapper.sh`).
You need to source this in your shell startup script. If you are on a Mac and use
bash for your shell (the default), this command should work:

```console
$ echo 'source /usr/local/bin/virtualenvwrapper.sh' >> ~/.bash_profile
```
Now, you can start using virtualenv. You may have to restart your terminal window.

To create a new virtualenv named 'project1':

```console
$ mkvirtualenv project1
...
(project1) $
```
Of course, feel free to name your virtualenv whatever you want. Notice that
virtualenv modified your shell prompt so that the name of your active
virtualenv is conveniently displayed. Now, whenever you run any Python scripts,
you will be running it against the Python in your virtualenv, so feel free
to install any packages you want.

What `mkvirtualenv` does is put your virtualenv into `~/.virtualenvs`. To switch
to a different virtualenv, use the command `workon`.

```console
$ workon
project1
(other virtualenvs here)
$ workon project1
(project1) $
```
To stop using a virtualenv, use the command `deactivate`.

```console
(project1) $ deactivate
...
$
```

Lastly, to remove a virtualenv, use the command `rmvirtualenv`:

```console
$ rmvirtualenv project1
```

## Approach 2: Manually Using Virtualenv
If you do not want to install an additional package, or perhaps you like putting
your project's virtualenv right in your project directory, you can use virtualenv
directly.

From your project directory (or wherever you want to put your virtualenv), run
this command:

```console
$ virtualenv venv
```
This will create a new folder named `venv` in your current directory, and put
your virtualenv there. Of course, feel free to name this folder anything you want.

To start using your virtualenv, you have to source the `bin/activate` script.

```console
$ source venv/bin/activate
(venv) $
```
Virtualenv uses the name of the folder as the name of your virtualenv and modifies
your prompt to display the active virtualenv.

You now have a virtual Python environment where you can safely install any
packages you want!

To stop using a virtualenv, use the command `deactivate`.

```console
(venv) $ deactivate
...
$
```
Removing a virtualenv is as simple as removing the directory you created
the virtualenv with:

```console
$ rm -rf venv
```

# When Do I Use Virtualenv?
**Use virtualenv whenever humanly possible!**

The rule of thumb is to only install packages to your global Python environment
that you do not mind upgrading. These packages include virtualenv
itself and any tools you use everyday, perhaps as part of your text editor's plugin.
I keep both jedi and flake8 in my global Python environment. Most importantly,
none of your projects should depend on anything in your global Python environment.
