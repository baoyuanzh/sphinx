===============
出发
===============

一旦 Sphinx 已 :doc:`安装 </usage/installation>` 完成, 你就可以开始设置你的第一个 Sphinx 项目了。
为简化启动流程，Sphinx 提供了一个实用工具 —— :program:`sphinx-quickstart`, 它将帮助你生成一个文档源目录，并在这一过程中提供一些默认参数。
在这里，我们将演示如何使用该工具（ :program:`sphinx-quickstart` ），尽管这一步并不是必须的。


设置文档源
------------------------------------

包含一众 Sphinx 的 :term:`reStructuredText` 源文件集的根目录被称为 :term:`source directory` （源目录）。
该目录同时也包含了 Sphinx 的配置文件 :file:`conf.py` ，在这里你可以配置 Sphinx 的方方面面，使 Sphinx 按照你的要求读取源文件并创建文档。 [#]_

Sphinx 提供一个叫做 :program:`sphinx-quickstart` 的脚本，它将帮助你建立一个源目录 (source directory) ，并在其中创建默认的配置文件 :file:`conf.py` 。
它通过几个简单的问题获取一些最有用的配置值。
你只需要运行

.. code-block:: shell

   $ sphinx-quickstart

来回答每一个问题。
关于 ``autodoc`` 扩展的问题，请确保回答 yes , 因为稍后我们将使用到它。

与之相关的有一个叫做 :program:`sphinx-apidoc` 的 “API 文档” 自动生成器；请参考 :doc:`/man/sphinx-apidoc` 以获取详细信息。


定义文档结构
---------------------------

假定你已经运行了 :program:`sphinx-quickstart` 。
它创建了一个包含配置文件 :file:`conf.py` 和一个主文档 :file:`index.rst` 的源目录（如果你接受了默认选项）。
主文档 (:term:`master document`) 的主要作用乃是作为一个导航欢迎页面，它包含一个“根目录表”（ table of contents
tree (或 *toctree*) ）。
这是 Sphinx 对 reStructuredText 的一项主要扩展。
Sphinx 通过这种方式，将众多源文件与一个集中的层级结构相关联。

.. sidebar:: reStructuredText directives （指令）

   ``toctree`` 是 reStructuredText 的一个指令（ :dfn:`directive` ），一种用途十分广泛的标记。
   指令（ Directives ）可以有参数（ Arguments ）、选项（ Options ）和内容（ Contents ）。

   *参数*（ *Arguments* ）直接在紧跟指令名称的双冒号后面给出。
   每一个指令决定自身是否可以有参数，有几个参数。

   *选项*（ *Options* ）则跟在参数后面，以“字段列表”（ "field list" ）的形式给出。
   比如 ``maxdepth`` 就是指令 ``toctree`` 的一个选项。

   *内容*（ *Content* ）在最后给出，并与选项或参数（没有选项时）隔一个空行。
   每一个指令决定自身是否允许有内容，以及要如何处理内容。

   关于指令，一个容易出错的地方是，我们约定 **内容的第一行应与选项保持相同的缩进**。

指令 ``toctree`` 一开始是空的，就像下面这样：

.. code-block:: rest

   .. toctree::
      :maxdepth: 2

在指令内容（ *content* ）的位置，你可以通过添加文件的相对路径及名称来添加文档，如下所示，

.. code-block:: rest

   .. toctree::
      :maxdepth: 2

      usage/installation
      usage/quickstart
      ...

这也正是本文档指令 ``toctree`` 的样子。
所有要被添加的文档都以如下“路径加名称” :term:`document name`\ s 的方式被列出。
简单说，文档名去掉后缀，路径则以反斜线作为目录的分隔符。


|more| 更多内容请参考 :ref:`the toctree directive <toctree-directive>`.

现在，你可以创建被指令 ``toctree`` 列出的文档了，并向这些文档添加内容。
文档的章节标题（ 直到由 maxdepth 指定的深度）都将被插入到指令 ``toctree`` 的位置上去。

此时， Sphinx 就已经明白了你的项目中各个文档的顺序关系与其中的层级结构。（这些被插入的文档也可以有自己的 ``toctree`` 指令，也就是说，如有必要，通过这种方式，你可以创建嵌套很深的层级结构。）


添加内容
--------------

在 Sphinx 源文件中，你可以使用标准 :term:`reStructuredText` 的大多数特性。
而其他一些特性则是由 Sphinx 添加的。
例如，你可以用 :rst:role:`ref` 角色（role）以一种可移植的方式添加跨文件的交叉引用（对所有输出类型均有效）。

再比如，在阅读 HTML 类型的输出中，如果你想查看当前文档的源文件，只需要点击边框栏中的 "Show Source" （“显示源代码”）即可。

.. todo:: 这部分添加新的指南后，更新一下链接。

|more| 请参考 :doc:`/usage/restructuredtext/index` ，这里关于 reStructuredText 的介绍更深入，并包括由 Sphinx 添加的标记。


运行 build 程序
-----------------

Now that you have added some files and content, let's make a first build of the
docs.  A build is started with the :program:`sphinx-build` program:

.. code-block:: shell

   $ sphinx-build -b html sourcedir builddir

where *sourcedir* is the :term:`source directory`, and *builddir* is the
directory in which you want to place the built documentation.
The :option:`-b <sphinx-build -b>` option selects a builder; in this example
Sphinx will build HTML files.

|more| Refer to the :program:`sphinx-build man page <sphinx-build>` for all
options that :program:`sphinx-build` supports.

However, :program:`sphinx-quickstart` script creates a :file:`Makefile` and a
:file:`make.bat` which make life even easier for you. These can be executed by
running :command:`make` with the name of the builder. For example.

.. code-block:: shell

   $ make html

This will build HTML docs in the build directory you chose. Execute
:command:`make` without an argument to see which targets are available.

.. admonition:: How do I generate PDF documents?

   ``make latexpdf`` runs the :mod:`LaTeX builder
   <sphinx.builders.latex.LaTeXBuilder>` and readily invokes the pdfTeX
   toolchain for you.


.. todo:: Move this whole section into a guide on rST or directives

为 objects 编写说明
-------------------

One of Sphinx's main objectives is easy documentation of :dfn:`objects` (in a
very general sense) in any :dfn:`domain`.  A domain is a collection of object
types that belong together, complete with markup to create and reference
descriptions of these objects.

The most prominent domain is the Python domain. For example, to document
Python's built-in function ``enumerate()``, you would add this to one of your
source files.

.. code-block:: restructuredtext

   .. py:function:: enumerate(sequence[, start=0])

      Return an iterator that yields tuples of an index and an item of the
      *sequence*. (And so on.)

This is rendered like this:

.. py:function:: enumerate(sequence[, start=0])

   Return an iterator that yields tuples of an index and an item of the
   *sequence*. (And so on.)

The argument of the directive is the :dfn:`signature` of the object you
describe, the content is the documentation for it.  Multiple signatures can be
given, each in its own line.

The Python domain also happens to be the default domain, so you don't need to
prefix the markup with the domain name.

.. code-block:: restructuredtext

   .. function:: enumerate(sequence[, start=0])

      ...

does the same job if you keep the default setting for the default domain.

There are several more directives for documenting other types of Python
objects, for example :rst:dir:`py:class` or :rst:dir:`py:method`.  There is
also a cross-referencing :dfn:`role` for each of these object types.  This
markup will create a link to the documentation of ``enumerate()``.

::

   The :py:func:`enumerate` function can be used for ...

And here is the proof: A link to :func:`enumerate`.

Again, the ``py:`` can be left out if the Python domain is the default one.  It
doesn't matter which file contains the actual documentation for
``enumerate()``; Sphinx will find it and create a link to it.

Each domain will have special rules for how the signatures can look like, and
make the formatted output look pretty, or add specific features like links to
parameter types, e.g. in the C/C++ domains.

|more| See :doc:`/usage/restructuredtext/domains` for all the available domains
and their directives/roles.


基本配置
-------------------

Earlier we mentioned that the :file:`conf.py` file controls how Sphinx
processes your documents.  In that file, which is executed as a Python source
file, you assign configuration values.  For advanced users: since it is
executed by Sphinx, you can do non-trivial tasks in it, like extending
:data:`sys.path` or importing a module to find out the version you are
documenting.

The config values that you probably want to change are already put into the
:file:`conf.py` by :program:`sphinx-quickstart` and initially commented out
(with standard Python syntax: a ``#`` comments the rest of the line).  To
change the default value, remove the hash sign and modify the value.  To
customize a config value that is not automatically added by
:program:`sphinx-quickstart`, just add an additional assignment.

Keep in mind that the file uses Python syntax for strings, numbers, lists and
so on.  The file is saved in UTF-8 by default, as indicated by the encoding
declaration in the first line.  If you use non-ASCII characters in any string
value, you need to use Python Unicode strings (like ``project = u'Exposé'``).

|more| See :ref:`build-config` for documentation of all available config values.


.. todo:: Move this entire doc to a different section

Autodoc
-------

When documenting Python code, it is common to put a lot of documentation in the
source files, in documentation strings.  Sphinx supports the inclusion of
docstrings from your modules with an :dfn:`extension` (an extension is a Python
module that provides additional features for Sphinx projects) called *autodoc*.

In order to use *autodoc*, you need to activate it in :file:`conf.py` by
putting the string ``'sphinx.ext.autodoc'`` into the list assigned to the
:confval:`extensions` config value.  Then, you have a few additional directives
at your disposal.

For example, to document the function ``io.open()``, reading its signature and
docstring from the source file, you'd write this::

   .. autofunction:: io.open

You can also document whole classes or even modules automatically, using member
options for the auto directives, like ::

   .. automodule:: io
      :members:

*autodoc* needs to import your modules in order to extract the docstrings.
Therefore, you must add the appropriate path to :py:data:`sys.path` in your
:file:`conf.py`.

.. warning::

   :mod:`~sphinx.ext.autodoc` **imports** the modules to be documented.  If any
   modules have side effects on import, these will be executed by ``autodoc``
   when ``sphinx-build`` is run.

   If you document scripts (as opposed to library modules), make sure their
   main routine is protected by a ``if __name__ == '__main__'`` condition.

|more| See :mod:`sphinx.ext.autodoc` for the complete description of the
features of autodoc.


.. todo:: Move this doc to another section

Intersphinx
-----------

Many Sphinx documents including the `Python documentation`_ are published on
the internet.  When you want to make links to such documents from your
documentation, you can do it with :mod:`sphinx.ext.intersphinx`.

.. _Python documentation: https://docs.python.org/3

In order to use intersphinx, you need to activate it in :file:`conf.py` by
putting the string ``'sphinx.ext.intersphinx'`` into the :confval:`extensions`
list and set up the :confval:`intersphinx_mapping` config value.

For example, to link to ``io.open()`` in the Python library manual, you need to
setup your :confval:`intersphinx_mapping` like::

   intersphinx_mapping = {'python': ('https://docs.python.org/3', None)}

And now, you can write a cross-reference like ``:py:func:`io.open```.  Any
cross-reference that has no matching target in the current documentation set,
will be looked up in the documentation sets configured in
:confval:`intersphinx_mapping` (this needs access to the URL in order to
download the list of valid targets).  Intersphinx also works for some other
:term:`domain`\'s roles including ``:ref:``, however it doesn't work for
``:doc:`` as that is non-domain role.

|more| See :mod:`sphinx.ext.intersphinx` for the complete description of the
features of intersphinx.


更多主题
-------------------------

- :doc:`其他扩展 </extensions>`:

  * :doc:`/ext/math`,
  * :doc:`/ext/viewcode`,
  * :doc:`/ext/doctest`,
  * ...
- Static files
- :doc:`挑选一个主题风格 </theming>`
- :doc:`/setuptools`
- :ref:`模板 <templating>`
- 应用扩展
- :ref:`编写扩展 <dev-extensions>`


.. rubric:: Footnotes

.. [#] 这是通常的布局。当然, :file:`conf.py` 也可以存放在由 :term:`configuration directory` 指定的其他目录中。请参考 :program:`sphinx-build man page <sphinx-build>` 以获取更多信息。

.. |more| image:: /_static/more.png
          :align: middle
          :alt: more info
