---
tags: [tutorials]
---
{% capture part_2_url %}{% post_url 2016-07-07-using-sphinx,-part-2:-writing-documentation %}{% endcapture %}
This is the first of a two-parts tutorial series on how you can use [Sphinx](http://www.sphinx-doc.org/) to write professional documentation for your codebase. Sphinx is a tool written in Python that converts documents written in reStructuredText into an output of your choice (HTML, latex, pdf, etc.). In this first part, I will show how to add Sphinx to an existing codebase. [Part 2]({{ part_2_url }}) will focus on how to write documentation using Sphinx.

**Note**: Sphinx is not limited to documenting Python code. That said, it has excellent integration with Python, able to automatically read docstrings in your code and generate documentation. There will be a separate tutorial on how to do that soon.

**Note**: This tutorial was written with Sphinx 1.4.4.

# Installing Sphinx
Sphinx is distributed using Pypi, so even if your project is not a Python project, you will have to use pip. I highly recommend using virtualenv so that you can lock in a version of Sphinx used with your particular project. Virtualenv is very simple to set up; see my tutorial [here]({% post_url 2016-06-30-using-virtualenv %}).

Inside your virtualenv (make sure you create a new one for this project!), run:

```console
$ pip install sphinx
```

# Adding Sphinx To Your Project
If you have an existing project, cd into the project directory. If you just want to follow along with this tutorial, you can just do so from any directory. Run the Sphinx setup script:

```console
$ sphinx-quickstart
```

This will run an interactive script that will ask you a bunch of questions about your project. How you answer should not affect this tutorial too much, but here is the transcript of my run, omitting wherever I stuck with the default option:

```console
...
> Root path for the documentation [.]: docs
...
> Project name: project name
> Author name(s): Naphat Sanguansin
...
> Project version: 1.0.0
...
> autodoc: automatically insert docstrings from modules (y/n) [n]: y
...
```

Using autodoc (the last visible option in my run transcript) is optional, but recommended if you have a Python project. It will not be needed to complete this tutorial.

# Compiling Your Docs
If you used the default options in the setup script, then the script would have created a Makefile for you. To view the result of your hard work, cd into your documentations folder and run:

```console
$ make html
```

Then open up the file `_build/html/index.html`.

![initial-docs](/assets/tutorials/using-sphinx-to-write-documentation/initial.png){:class="img-responsive"}

Congratulations! You are on your way to a professional documentation page.

Sphinx has a bunch of other output formats preconfigured for you. To see what they are, run `make help`.

# Automatically Compiling Your Docs
When writing your documentation, it can be a pain to have to rerun the make command every time you make a change. Fortunately for us, there is a Python package that automatically watches your working directory for changes and recompiles your documentation. This package is `sphinx-autobuild`.

```console
$ pip install sphinx-autobuild
```

To use it, stick this into your `Makefile` and run `make livehtml`.

```makefile
livehtml:
	sphinx-autobuild -b html $(ALLSPHINXOPTS) $(BUILDDIR)/html
```

You'll want to make sure that you have a tab character (not 4 space characters) on the line after `livehtml:`, otherwise make will not parse this correctly. Be sure to put this somewhere after `ALLSPHINXOPTS` and `BUILDDIR` is defined in the Makefile - a good option is to put it after the `html` target (the block starting with `html:`).

When you run `make livehtml`, the command will block and a web server will be started. To access your documentation, go to <http://localhost:8000>{:target="_blank"}.

To stop the server, hit `ctrl-c`.

**Note**: `sphinx-autobuild` is convenient, but not perfect. It does not properly detect dependencies across different documents. For example, if `A.rst` depends on `B.rst`, and `B.rst` is changed, `sphinx-autobuild` will only recompile `B.rst`, not `A.rst`. When in doubt, stop the server, run `make clean`, and start the server again.

# Requirements.txt
Your `docs/` subdirectory should be treated as a distinct Python project from your parent project, even if your parent project is not itself a Python project. Like all good Python projects, you should lock in the version of Python packages you are using. One way to do this is through a `requirements.txt` file. Run this command inside your virtualenv, in your `docs/` subdirectory:

```console
$ pip freeze | \grep -i sphinx > requirements.txt
```

If the above command fails for some reason, you can create a `requirements.txt` manually. To do that, run `pip freeze` and copy the lines containing `sphinx` and `sphinx-autobuild`. Then, create a new file `requirements.txt` in your `docs/` subdirectory, pasting in the copied lines.

For reference, at the time of this writing, my `requirements.txt` looks like this:

```
Sphinx==1.4.4
sphinx-autobuild==0.6.0
```

Whenever someone new clones your project, or if you need to get a new virtualenv, you can run the following command from a clean virtualenv:

```console
$ pip install -r requirements.txt
```

# Where To Go From Here?
You have now added a bare Sphinx project to your codebase. If you are using version control, now is a good time to check the Sphinx project into version control. Then, head over to [Part 2]({{ part_2_url }}) to see how can you write documentation using Sphinx.
