===============
出发
===============

一旦 Sphinx 已 :doc:`安装 </usage/installation>` 完成, 你就可以开始设置你的第一个 Sphinx 项目了。
为简化启动流程，Sphinx 提供了一个实用工具 —— :program:`sphinx-quickstart`, 它将帮助你生成一个文档源目录，并在这一过程中提供一些默认参数。
在这里，我们将演示如何使用该工具 (:program:`sphinx-quickstart`) ，尽管这一步并不是必须的。


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
主文档 (:term:`master document`) 的主要作用乃是作为一个导航欢迎页面，它包含一个“根目录表” (table of contents
tree (或 *toctree*))。
这是 Sphinx 对 reStructuredText 的一项主要扩展。
Sphinx 通过这种方式，将众多源文件与一个集中的层级结构相关联。

.. sidebar:: reStructuredText directives （指令）

   ``toctree`` 是 reStructuredText 的一个指令 (:dfn:`directive`) ，一种用途十分广泛的标记。
   指令 (Directives) 可以有参数 (Arguments) 、选项 (Options) 和内容 (Contents) 。

   *参数* (*Arguments*) 在紧跟指令名称的双冒号后面直接给出。
   每一个指令决定自身是否可以有参数，有几个参数。

   *选项* (*Options*) 则跟在参数后面，以“字段列表” ("field list") 的形式给出。
   比如 ``maxdepth`` 就是指令 ``toctree`` 的一个选项。

   *内容* (*Content*) 在最后给出，并与选项或参数（没有选项时）隔一个空行。
   每一个指令决定自身是否允许有内容，以及要如何处理内容。

   关于指令，一个容易出错的地方是，我们约定 **内容的第一行应与选项保持相同的缩进**。

指令 ``toctree`` 一开始是空的，就像下面这样：

.. code-block:: rest

   .. toctree::
      :maxdepth: 2

在指令内容 (*content*) 的位置，你可以通过添加文件的相对路径及名称来添加文档，如下所示，

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
文档的章节标题（直到由 maxdepth 指定的深度）都将被插入到指令 ``toctree`` 的位置上去。

此时， Sphinx 就已经明白了你的项目中各个文档的顺序关系与其中的层级结构。（这些被插入的文档也可以有自己的 ``toctree`` 指令，也就是说，如有必要，通过这种方式，你可以创建嵌套很深的层级结构。）


添加内容
--------------

在 Sphinx 源文件中，你可以使用标准 :term:`reStructuredText` 的大多数特性。
而其他一些特性则是由 Sphinx 添加的。
例如，你可以用 :rst:role:`ref` 角色 (role) 以一种可移植的方式添加跨文件的交叉引用（对所有输出类型均有效）。

再比如，在阅读 HTML 类型的输出中，如果你想查看当前文档的源文件，只需要点击边框栏中的 "Show Source" （“显示源代码”）即可。

.. todo:: 这部分添加新的指南后，更新一下链接。

|more| 请参考 :doc:`/usage/restructuredtext/index` ，这里关于 reStructuredText 的介绍更深入，并包括由 Sphinx 添加的标记。


构建 (build) 文档
-----------------

现在假定你已经添加了一些源文件并有了一定的内容，让我们一起来看看在此基础上如何构建文档。
构建过程从调用 :program:`sphinx-build` 工具开始：

.. code-block:: shell

   $ sphinx-build -b html sourcedir builddir

其中， *sourcedir* 是源目录 (:term:`source directory`) ， *builddir* 则是用来存放输出文档的目录。
选项 :option:`-b <sphinx-build -b>` 选择一个构建类型；在这个例子中，Sphinx 将生成 HTML 类型的文档。

|more| 请参考 :program:`sphinx-build man page <sphinx-build>` 以了解 :program:`sphinx-build` 所支持的所有选项。

不过，由 :program:`sphinx-quickstart` 脚本创建的 :file:`Makefile` 文件和 :file:`make.bat` 文件将使得构建文档这一操作变得更加简单。
你可以用命令 :command:`make` 指定构建类型来创建文档。
比如，

.. code-block:: shell

   $ make html

该命令将在你选择的文档输出目录中构建 HTML 文档。
不添加任何构建类型来运行 :command:`make` 命令可以查看有哪些可选的类型。

.. admonition:: 如何生成 PDF 文档？

   ``make latexpdf`` 运行 :mod:`LaTeX builder <sphinx.builders.latex.LaTeXBuilder>` 并为你无缝调用 pdfTeX 的工具链。


.. todo:: 将本节全部移至 rST 的一个指南或者 directives 中。

为对象 (objects) 编写文档
-------------------

Sphinx 的一个主要目标是能够容易地为任何 :dfn:`domain` （领域）中，（极为宽泛意义下的对象） :dfn:`objects` 编写文档。
领域 (domain) 是众多对象类型 (object types) 的汇总，它们都属于这个整体。
作为完整的对象，它们有用以创建的标记和可以引用参考的说明。

一个最为突出的领域是 Python 域。
例如，要为 Python 的內建函数 ``enumerate()`` 编写文档，你可以把如下的几行文字添加到某个源文件中。

.. code-block:: restructuredtext

   .. py:function:: enumerate(sequence[, start=0])

      Return an iterator that yields tuples of an index and an item of the
      *sequence*. (And so on.)

上面的文字将被渲染成如下样式：

.. py:function:: enumerate(sequence[, start=0])

   Return an iterator that yields tuples of an index and an item of the
   *sequence*. (And so on.)

以上指令（ directive ）的参数是你要描述的对象 (object) 的签名 (:dfn:`signature`) ，指令的内容则是对象 (object) 的文档。
可以指定多个签名 (signatures) ，一个一行。

事实上， Python 域就是 Sphinx 的默认域，所以作为指令前缀的域的名称并不是必要的，

.. code-block:: restructuredtext

   .. function:: enumerate(sequence[, start=0])

      ...

如果你保持默认设定和默认域不变，那么省去前缀的这个写法和完整写法的效果是一样的。

为 Python 其他类型的对象编写文档，我们需要更多的指令 (directives) 。
比如有 :rst:dir:`py:class` 和 :rst:dir:`py:method`。
针对每一个这样的对象类型，我们还有一些对应的用于交叉引用的角色 (:dfn:`role`) 。
如下标记将创建一个跳转到函数 ``enumerate()`` 的文档的一个链接。

::

   The :py:func:`enumerate` function can be used for ...

这里有一个演示：请尝试点击 :func:`enumerate`.

同样的, 在 Python 域是默认域的前提下，前缀 ``py:`` 可以省略。
具体是哪一个源文件包含了函数 ``enumerate()`` 的文档并不重要；
重要的是 Sphinx 能够找到它并自动创建指向它的链接。

每一个领域 (domain) 各自都有一些特殊的规则。
比如关于合法的签名长什么样子，如何让渲染输出的格式更美观，以及一些独有的特性，像是可以生成跳转到参数类型的文档的链接，例如在 C/C++ 域中就有这样的特性。

|more| 请参考 :doc:`/usage/restructuredtext/domains` 以便获取所有可选的域 (domains) ，以及域中相关指令/角色 (directives/roles) 的详细信息。


基本配置
-------------------

前面提到的配置文件 :file:`conf.py` 控制着 Sphinx 如何生成你的文档。
而该文件其实是被当做 Python 源文件来运行你所指定的配置信息的。
对于高级用户而言：由于配置文件可以被 Sphinx 运行的，你实际上可以完成一些复杂的任务。
比如，扩展 :data:`sys.path` 或者导入一个模块来找出你正在编写文档的版本。

由 :program:`sphinx-quickstart` 生成的配置文件通常已包含大多数你想要修改的配置，只不过它们最初是被注释起来的（用的是标准的 Python 的语法：即以 ``#`` 注释掉当前行的剩余部分）。
要修改默认值，只要去掉井号，然后替换成你期望值即可。
要自定义一个 :program:`sphinx-quickstart` 没有自动生成的，新的配置，添加一个赋值语句就可以了。

要记住的是，该配置文件所使用的字符串、数字、列表等等都要符合 Python 的语法。
此外，如第一行编码声明（实际观察发现是在第二行）所示，

::

   # -*- coding: utf-8 -*-

文件默认以 UTF-8 编码保存。
如果你要使用非 ASCII 字符的字符串，那就得用 Python Unicode 字符串 (像这样 ``project = u'Exposé'``) 。

|more| 请参考 :ref:`build-config` 以获取所有可选配置的信息。


.. todo:: 将这一部分完整转移到另一个章节

自动文档 (Autodoc)
------------------

在给 Python 代码编写文档时，直接把大量的文档以文档字符串 (documentation strings) 的形式写在源代码文件中是非常常见的做法。
Sphinx 中有一个叫做 *autodoc* 的扩展 (:dfn:`extension` 一个扩展就是为 Sphinx 项目提供附加功能的一个 Python 模块) 支持从你的模块中自动抓取这些文档字符串 (docstrings)。

要使用 *autodoc* ，你必须在配置文件 :file:`conf.py` 中启用它。
启用的方式是在配置项 :confval:`extensions` 列表里写上字符串 ``'sphinx.ext.autodoc'`` 。
如此，你就有一些额外的相关指令可用了。

比如，要给函数 ``io.open()`` 添加文档，只需这样写，你就可以从源代码文件中读取它的签名以及文档字符串 ::

   .. autofunction:: io.open

你也可以通过使用带成员选项 (member oprtions) 的 auto 指令 (directives) 为一整个类或一整个模块自动提取文档，像这样 ::

   .. automodule:: io
      :members:

为了能提取文档字符串 (docstrings) ， *autodoc* 需要导入你的模块。
因此，你必须在你的配置文件 (:file:`conf.py`) 中给 :py:data:`sys.path` 添加适当的路径。

.. warning::

   要使用 :mod:`~sphinx.ext.autodoc` ，则必须先 **导入** 要被提取文档的模块。
   任何一个关于导入操作有副作用的模块，在 ``sphinx-build`` 运行时，这些副作用亦将被执行。

   如果你要给脚本（与库模块不同）添加文档，请确保它们的主程序被条件测试 ``if __name__ == '__main__'`` 保护起来了。

|more| 请参考 :mod:`sphinx.ext.autodoc` 以获取关于 autodoc 各项功能特性的完整描述。


.. todo:: 将本节内容转移至另一个章节

Intersphinx
-----------

很多用 Sphinx 编写的文档，包括 `Python documentation`_ 都发布在网上。
如果你想从自己的文档链接到那些文档的话，你可以利用该扩展 :mod:`sphinx.ext.intersphinx` 这样做，

.. _Python documentation: https://docs.python.org/3

要使用 intersphinx ，你必须在配置文件 :file:`conf.py` 中启用它，激活方式是在配置项 :confval:`extensions` 列表中添加字符串 ``'sphinx.ext.intersphinx'`` 并设定好配置 :confval:`intersphinx_mapping` 的值。

比如说，要链接到 Python library 的方法 ``io.open()`` 的手册，你需要将 :confval:`intersphinx_mapping` 配置成 ::

   intersphinx_mapping = {'python': ('https://docs.python.org/3', None)}

现在，你就可以用 ``:py:func:`io.open``` 来交叉引用了。
任何在当前的文档汇总中没有匹配的交叉引用都将在配置 :confval:`intersphinx_mapping` 设定的链接中查找（当然这需要访问链接来下载合法目标的列表）。
也可以用 Intersphinx 来引用领域 (:term:`domain`) 角色 (roles)，包括 ``:ref:`` 角色，但却不能引用 ``:doc:`` ，因为这不是一个领域角色。

|more| 请参考 :mod:`sphinx.ext.intersphinx` 以了解 intersphinx 各项功能的完整描述。


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
