---
tags: [tutorials]
---
{% capture part_1_url %}{% post_url 2016-07-07-using-sphinx,-part-1:-setting-up %}{% endcapture %}
This is Part 2 of my tutorial series on using Sphinx to write professional documentation. This part focuses on how to write documentation after Sphinx has been set up. If you haven't done so already, be sure to check out [Part 1]({{ part_1_url }}), where I go over how to set up Sphinx in an existing codebase.

# Starter Code
If you followed [Part 1]({{ part_1_url }}), then you already have a starter Sphinx project and can skip this section.

This tutorial assumes that you already have Sphinx set up in your project. You can check out [Part 1]({{ part_1_url }}) of this tutorial series for how to do that in your own project, or you can clone my starter project:

```console
$ git clone https://github.com/naphatkrit/math-utils
$ cd math-utils
$ git checkout using-sphinx-2-starter
$ cd docs
$ pip install -r requirements.txt # make sure to only do this in a virtualenv
```

This will put you into the Sphinx project subdirectory, which is the only directory you have to worry about for this tutorial. If you don't know what virtualenv is, check out my short tutorial on it [here]({% post_url 2016-06-30-using-virtualenv %}).

# Running the Development Server
Before doing anything else, make sure you have the development server running:

```console
$ make livehtml # from inside docs/
```

This command blocks, so do it in a separate terminal window. Your documentation is now available at <http://localhost:8000/>{:target="_blank"}.

**Note**: `livehtml` is a custom make target set up in [Part 1]({{ part_1_url }}) and is also available in the starter code.

# Creating Your First Document

Let's create your first document. Inside your `docs/` folder, create a new file called `getting_started.rst`. This is what mine looks like:

```reStructuredText
Getting Started With (project name)
====================================

This is a document on how to get started using my project.
```

Now try going to <http://localhost:8000/getting_started.html>{:target="_blank"}.

![getting-started](/assets/tutorials/using-sphinx-to-write-documentation/getting_started.png){:class="img-responsive"}

Congratulations! You created your first document.

Unfortunately, this document is not accessible from the documentation home page. To fix that, take a look at the file `index.rst`. Somewhere in the file, you should see the structure:

```reStructuredText
.. toctree::
   :maxdepth: 2
```

This is the code that generates the table of content you see in your home page. Modify this to:

```reStructuredText
.. toctree::
   :maxdepth: 2

   getting_started
```

Then go to <http://localhost:8000>{:target="_blank"}. You should see your document added to the table of content.

![home-with-getting-started](/assets/tutorials/using-sphinx-to-write-documentation/toc_with_getting_started.png){:class="img-responsive"}

You are also free to organize your documents inside a subfolder. For instance, go ahead and move `getting_started.rst` into a new folder `getting_started/`, and rename the file to `index.rst`. This sequence of command will do it (do this from inside `docs/`):

```console
$ mkdir getting_started
$ mv getting_started.rst getting_started/index.rst
```

Then modify the table of content part in `index.rst` accordingly:

```reStructuredText
.. toctree::
   :maxdepth: 2

   getting_started/index
```

Go to <http://localhost:8000>{:target="_blank"}. Everything should work as before, except when you go to your Getting Started page, the url should say <http://localhost:8000/getting_started/index.html>{:target="_blank"} instead of <http://localhost:8000/getting_started.html>{:target="_blank"}.

# Referencing Other Documents

Very often, you will need to reference other documents when writing documentation. One way to do this is by putting labels into your document. In your `getting_started/index.rst` file, add a line right before your title, in the format `.. _LABEL:`, like this:

```reStructuredText
.. _getting_started/index:

Getting Started With (project name)
====================================

This is a document on how to get started using my project.
```
The extra newline character between the label and the title is important - it tells Sphinx to use the title as the default text whenever this label is referenced.

You can now reference this part of the document from anywhere. To see this in action, go to the root `index.rst` and add this paragraph anywhere:

```reStructuredText
This is a reference to getting started: :ref:`getting_started/index`.

This is another reference to getting started, with a custom title: :ref:`Custom Title <getting_started/index>`.
```

Now go to <http://localhost:8000>{:target="_blank"}. You should see two new links on the page, one with the title of your reference and the other one with the custom title "Custom Title".

![with-references](/assets/tutorials/using-sphinx-to-write-documentation/toc_with_references.png){:class="img-responsive"}

**Note**: If your links did not render correctly, try shutting down the development server, running `make clean`, and then starting the server back up.

Labels are not limited to the topmost title on the page; they can be added to any titles. To see this, modify your `getting_started/index.rst`:

```reStructuredText
.. _getting_started/index:

Getting Started With (project name)
====================================

This is a document on how to get started using my project.

.. _getting_started/index//subtitle_1:

Subtitle 1
+++++++++++
This is the content of subtitle 1.

.. _getting_started/index//subtitle_2:

Subtitle 2
+++++++++++
This is the content of subtitle 2.
```

Then add this to anywhere in your `index.rst`:

```reStructuredText
:ref:`getting_started/index//subtitle_1`

:ref:`Custom Title for Subtitle 2 <getting_started/index//subtitle_2>`
```

Then go to <http://localhost:8000>{:target="_blank"}, and see your new links. When you click on these links, they will take you to the exact position where your subtitles begin, using HTML ID.

![with-references-subtitles](/assets/tutorials/using-sphinx-to-write-documentation/toc_with_references_subtitles.png){:class="img-responsive"}

**Note**: You can name your label whatever you want, so use what makes sense for your project. The naming scheme I used in this tutorial is `path/to/file//subtitle`.

# Table of Contents
You have already seen an example of how to create a table of contents in `index.rst`:

```reStructuredText
.. toctree::
   :maxdepth: 2

   getting_started/index
```

What makes `toctree` really powerful is that you can create a table of contents anywhere, with any set of contents. For instance, a pattern I use a lot is to have the top level table of contents in `index.rst`, then for each subdirectory in my documentation, have a table of contents for that directory's `index.rst`. Let's go ahead and set up something similar.

Create a new file in `getting_started/`. Let's called it `getting_started/prerequisites.rst`.

```reStructuredText
.. _getting_started/prerequisites:

Prerequisites
==============

This is where you will learn to install the prereqs.
```

Then, open up `getting_started/index.rst` and replace the content of that file with:

```reStructuredText
.. _getting_started/index:

Getting Started
===============

This is a document on how to get started using my project.

.. toctree::

   /getting_started/prerequisites.rst
```

Go to <http://localhost:8000/getting_started/index.html>{:target="_blank"} and notice how you now have a table of contents inside your getting started page.

![getting-started-toc](/assets/tutorials/using-sphinx-to-write-documentation/getting_started_toc.png){:class="img-responsive"}

Now go to <http://localhost:8000/>{:target="_blank"} and notice something even more remarkable: your top-level table of contents have expanded to include the new prerequisites page!

![toc-nested](/assets/tutorials/using-sphinx-to-write-documentation/toc_nested.png){:class="img-responsive"}

The top-level `toctree` directive actually went inside `getting_started/index.rst`, saw that there is another `toctree` directive in there, and pulled out the content. This all happened without you doing any work!

If you recall, in the top-level `toctree` directive, there is an option, `maxdepth`. This tells Sphinx how many layers deep to pull out the content of nested `toctree` directives. Try changing the value to 1, and notice how the top-level table of contents no longer includes the prerequisites page. You should adjust this value to your liking.

# ReStructuredText/Sphinx Syntax
You have learned how to create a new document and saw some inter-document features of Sphinx.. In this section, we will go over some basic things you can do with reStructuredText and Sphinx within a single document.

## Basic ReStructuredText Formatting
For basic formatting like titles, bold texts, and comments, please see this excellent [cheatsheet](http://docutils.sourceforge.net/docs/user/rst/quickref.html){:target="_blank"} on reStructuredText formatting.

## Inline Code
To have Sphinx format text as inline code, use this syntax:

```reStructuredText
This is a normal paragraph with inline code :code:`here`.
```

This will be rendered as:
![inline-code](/assets/tutorials/using-sphinx-to-write-documentation/inline_code.png){:class="img-responsive"}

## Block Code
If you need to insert a whole chunk of code, you will want to use the block code format, like this:

```reStructuredText
.. code-block:: none

    Your code goes here
```

This will render the block of code without any syntax highlighting:

![block-code-none](/assets/tutorials/using-sphinx-to-write-documentation/block_code_none.png){:class="img-responsive"}

You can also have tell Sphinx what language your code is in, for proper syntax highlighting. For example, here is a block of Python code:

```reStructuredText
.. code-block:: python

    def subtract(x, y):
        return x - y
```

This will render with Python syntax highlighting:
![block-code-python](/assets/tutorials/using-sphinx-to-write-documentation/block_code_python.png){:class="img-responsive"}

Sphinx uses [Pygment](http://pygments.org/){:target="_blank"} for syntax highlighting. To see what language are supported, see [here](http://pygments.org/docs/lexers/){:target="_blank"}.

## Notes/Warnings

One very common thing you may want to do when writing documentation is to put a big giant blob of text that says "note" or "warning". Sphinx makes it very easy to do that:

```reStructuredText
This is regular text.

.. note::
    Here is some note.

    Here is some more note.

.. warning::
    Here is some warning.

    Here is some more warning.
```

Sphinx will render this beautifully and prominently so that your users can't miss them:
![notes-and-warnings](/assets/tutorials/using-sphinx-to-write-documentation/notes_and_warnings.png){:class="img-responsive"}

Sphinx has more options than notes and warnings. See the [docs](http://www.sphinx-doc.org/en/stable/markup/para.html){:target="_blank"} for the full list.

## Links
Sphinx automatically searches for anything that looks like a link, and make it clickable. For example, consider the following document:

```reStructuredText
Link to google: https://google.com
```

This will be rendered with the term "https://google.com" clickable:

![link](/assets/tutorials/using-sphinx-to-write-documentation/link_default.png){:class="img-responsive"}

You can also customize what your link is displayed as. Use the syntax `` `Display Name <http://example.com>`_ ``. For example, consider the following:

```reStructuredText
Link to google: `Google <https://google.com>`_
```

This will be rendered with "Google" clickable:

![link-custom](/assets/tutorials/using-sphinx-to-write-documentation/link_custom.png){:class="img-responsive"}

## Images
Inserting an image is easy:

```reStructuredText
.. image:: /images/my-image.png
```

![image-full](/assets/tutorials/using-sphinx-to-write-documentation/image_full.png){:class="img-responsive"}

Replace `/images/my-image.png` with the actual path to your image, with respect to your Sphinx project directory. You can also customize the size of your image:

```reStructuredText
.. image:: /images/my-image.png
    :width: 100px
```

![image-width](/assets/tutorials/using-sphinx-to-write-documentation/image_width.png){:class="img-responsive"}

The height can be controlled similarly.

# Selecting a Theme
So far, we have been using the default theme, `alabaster`. Sphinx has a bunch of built-in themes; see [here](http://www.sphinx-doc.org/en/stable/theming.html#builtin-themes){:target="_blank"} for the full list. To change your project's theme, open up `conf.py`, and look for `html_theme`. Change the value to:

```python
html_theme = 'classic'
```

You will also have to install this theme with pip:

Then go to <http://localhost:8000>{:target="_blank"} and see your documentation look completely different, all from just a single line change.

![classic](/assets/tutorials/using-sphinx-to-write-documentation/classic.png){:class="img-responsive"}

It is also possible to create custom themes, but doing so is beyond the scope of this tutorial. I recommend you check out the [official documentation](http://www.sphinx-doc.org/en/stable/theming.html#creating-themes){:target="_blank"} if you are interested.

# Where To Go From Here?
We have covered the basics of Sphinx in this tutorial series. You are now fluent enough in Sphinx to write professional documentation and use the [official Sphinx documentation](http://www.sphinx-doc.org/en/stable/contents.html){:target="_blank"} to fill in any gaps. Have fun writing!
