.. highlightlang:: rst

.. _rst-primer:

=======================
reStructuredText 初识
=======================

reStructuredText 是 Sphinx 默认使用的纯文本标记语言。 
本节是关于 reStructuredText (reST) 概念和语法的简要介绍，意在提供足够的信息以便作者能够高效地编写文档。
reST 被设计成是简单易用的标记语言，学习它不会花太长时间。

.. seealso::

   权威 `reStructuredText 用户手册 <http://docutils.sourceforge.net/rst.html>`_ 。
   本文档内 "ref" 链接可直接跳转到 reST 参考文献中相应内容的描述。


段落
----------

段落 (:duref:`ref <paragraphs>`) 是 reST 文档中最基本的块结构。
由一个或多个空行隔开的一段文本即为段落。
如同 Python 的语法规定，缩进在 reST 中也同样重要，同一个段落的文本必须由相同的缩进左对齐。


.. _rst-inline-markup:

行内标记
-------------

标准的 reST 行内标记很简单：

* 单星号对： ``*text*`` 意为强调（用斜体表示），
* 双星号对： ``**text**`` 意为重点强调（粗体表示），
* 反引号对： ````text```` 为代码样式。

如果星号或者反引号是行文文本，与做为标记符号产生混淆时，则需使用反斜杠转义。

注意行内标记使用上的一些限制：

* 不能嵌套使用，
* 内容与标记之间不能有空格： ``* text*`` 这是错误的
* 必须用 non-word 字符与周围文本隔开才能生效。可以用反斜杠转义空格符来绕过这一规则：像这样 ``thisis\ *one*\ word`` 。

在以后的 docutils 版本中，也许会移除这些限制。

用角色 (roles) 来代替或扩展这些行内标记是可能的。
更多信息请参考 :ref:`rst-roles-alt` 。


列表及类引用块
---------------------------

列表标记 (:duref:`ref <bullet-lists>`) 的使用形式是很自然的：只需要在每一段之前放一个星号并作适当缩进即可。
顺序列表也是一样，用井号 ``#`` 可以自动生成数字序号 ::

   * This is a bulleted list.
   * It has two items, the second
     item uses two lines.

   1. This is a numbered list.
   2. It has two items too.

   #. This is a numbered list.
   #. It has two items too.

嵌套列表也是可行的，但是注意，它们必须用一个空行与父列表项目隔开 ::

   * this is
   * a list

     * with a nested list
     * and some subitems

   * and here the parent list continues

定义列表 (:duref:`ref <definition-lists>`) 按如下方式构造 ::

   term (up to a line of text)
      Definition of the term, which must be indented

      and can even consist of multiple paragraphs

   next term
      Description.

注意一个要定义的术语名称不能超过一行。

段落引用 (:duref:`ref <block-quotes>`) 的构造只需要比周围段落多缩进一些即可。

行块 (line blocks) (:duref:`ref <line-blocks>`) 是用来保留断行文字的一种方式 ::

   | These lines are
   | broken exactly like in
   | the source file.

当然还有其他一些可用的块 (blocks) :

* 字段 (field) 列表 (:duref:`ref <field-lists>`, 及其注意事项 :ref:`rst-field-lists`)
* 选项 (options) 列表 (:duref:`ref <option-lists>`)
* 字面引用 (quoted literal) 块  (:duref:`ref <quoted-literal-blocks>`)
* 文档测试 (doctest) 块 (:duref:`ref <doctest-blocks>`)


.. _rst-literal-blocks:

字面块
--------------

字面代码块 (:duref:`ref <literal-blocks>`) 在段落结束的地方用一个特别的标记 ``::`` 引入。
字面代码块必须缩进，并且必须用一个空行与周围的段落隔开，就和其他所有的段落一样::

   这是一个普通文本段落。下一个段落是代码字面::

      It is not processed in any way, except
      that the indentation is removed.

      代码字面可以跨多行。

   这又是一个普通文本段落.

对于标记 ``::`` 本身的处理是很优雅的：

* 如果它本身作为一个段落出现，那么这个段落完全不会出现在文档中。
* 如果它前面有一个空格，那么标记不会出现在文档中（如同被移除）。
* 如果它前面是非空格字符，那么在文档中会用一个冒号来替代它。

因而，上例中，第一个段落的第二个句子将呈现为这样的形式，“下一个段落是代码字面：”。

在字面代码块中，代码高亮是可能的。
用指令 :rst:dir:`highlight` 可以在单个文件范围内启用代码高亮
要在项目范围内启用高亮，则需要设定 :confval:`highlight_language` 配置项。
而指令 :rst:dir:`code-block` 则可以在特定的段落上启用代码高亮。
这些指令稍后再讨论。


.. _rst-doctest-blocks:

Doctest 块
--------------

Doctest 块 (:duref:`ref <doctest-blocks>`) 表示一次与 Python 交互的对话，将被剪切复制到文档字符串中。
它不需要按照字面块 :ref:`literal blocks <rst-literal-blocks>` 的语法来写，但却是一个特殊的字面块。
Doctest 块必须以一个空行结束，且 *不能* 在文末跟随一个未使用的提示符::

    >>> 1 + 1
    2

.. _rst-tables:

表格
------

关于使用 *表格* (:duref:`ref <grid-tables>`) ，你得自己“画” 格子。像下面这样子::

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+

*简单表单* (:duref:`ref <simple-tables>`) 就容易多了，但形式比较受限：必须有超过一行的内容，且第一行列的格子内不能有多行内容，它看起来像这样::

   =====  =====  =======
   A      B      A and B
   =====  =====  =======
   False  False  False
   True   False  False
   False  True   False
   True   True   True
   =====  =====  =======

还有其他两种格式的表格： *CSV 表* and *列表 (List Tables)* .
它们要用 *显式的标记块 (explicit markup block)* 。
参考 :ref:`table-directives` 以获取更多信息。


超链接
----------

外部链接
~~~~~~~~~~~~~~

用 ```Link text <https://domain.invalid/>`_`` 表示行内的网络连接。
如果链接文字就是网址，那你就不需要使用任何特别的标记，因为 Sphinx 的分析器可以从普通文本中识别出网址和邮箱地址。

.. important:: 链接文字与包含 URL 的左开尖括号 \< 之间必须有一个空格。

你也可以将实际链接与目标定义分开放在两处 (:duref:`ref <hyperlink-targets>`)，比如这样::

   这段话包含 `a link`_.

   .. _a link: https://domain.invalid/

内部链接
~~~~~~~~~~~~~~

内部链接则需要使用由 Sphinx 提供的特殊 reST 角色来实现，请参看特殊标记那一节， :ref:`ref-role` 。


章节
---------

章节标题 (:duref:`ref <sections>`) 可以通过在标题名称下方重复用标点字符划线（在上方则可划可不划）的方式创建，且字符线至少和标题一样长::

   =================
   This is a heading
   =================

一般来讲，并没有特别规定哪种字符对应哪个级别的标题，因为标题的等级或者说文档的结构是按照标题出现的内在逻辑决定的。
当然，你也可以遵循 `Python 文档风格指南 <https://docs.python.org/devguide/documenting.html#style-guide>`_ 所指定的习惯用法:

* ``#`` 上下均划线表示 部
* ``*`` 上下均划线表示 章
* ``=``, 表示 节
* ``-``, 表示 子节
* ``^``, 表示 子子节
* ``"``, 表示 段落

当然，你可以自由选择你想用的标记字符（参考 reST 文档），并使用更深层次的嵌套等级，但值得注意的是大多数目标文档的格式 (HTML, LaTeX) 仅支持有限的嵌套深度。


.. _rst-field-lists:

Field 列表
-----------

Field 列表 (:duref:`ref <field-lists>`) 是按以下方式标记起来的一系列字段::

   :fieldname: Field content

在 Python 文档中它们很常用::

    def my_function(my_arg, my_other_arg):
        """A function just for me.

        :param my_arg: The first of my arguments.
        :param my_other_arg: The second of my arguments.

        :returns: A message (just for me, of course).
        """

Sphinx 扩展了 docutils 的标准行为并解释文档开头指出的 field 列表。
请参考 :doc:`field-lists` 以获得更多信息。


.. _rst-roles-alt:

角色
-----

角色或者说 "自定义如何解释的文本角色" (:duref:`ref <roles>`) 是一个显式标记的行内片段。
它指出被界定的文本需要用一种特别的方式对待。
于是 Sphinx 就用角色来提供语义标记以及交叉引用的标识，这些内容在对应的章节中说明。
角色的一般语法是这样的 ``:rolename:`content``` 。

Docutils 支持以下角色：

* :durole:`emphasis` -- 等同 ``*emphasis*``
* :durole:`strong` -- 等同 ``**strong**``
* :durole:`literal` -- 等同 ````literal````
* :durole:`subscript` -- 下标文本
* :durole:`superscript` -- 上标文本
* :durole:`title-reference` -- 书本，期刊和其他一些资料的名称

想要了解由 Sphinx 添加的角色请参考 :doc:`roles` 。


显式标记
---------------

“显式标记” (:duref:`ref <explicit-markup-blocks>`) 用在大多数 reST 中需要特殊处理结构，比如脚注，特别突出的段落，注释和一般指令。

一个显式标记块以 ``..`` 开头的一行开始，其后紧跟一个空格，并以下一个同等缩进级别的段落结束。
（在显式标记和普通段落之间需用一个空行隔开。这看起来似乎有点复杂，但真正用起来的时候其实很自然。）


.. _rst-directives:

指令
----------

指令 (:duref:`ref <directives>`) 是一个一般的显式标记块。
和角色 (roles) 一样，它是 reST 的扩展机制之一， Sphinx 极大的利用了这一点。

Docutils 支持以下指令：

* 警告： :dudir:`attention`, :dudir:`caution`, :dudir:`danger`,
  :dudir:`error`, :dudir:`hint`, :dudir:`important`, :dudir:`note`,
  :dudir:`tip`, :dudir:`warning` 和通用
  :dudir:`admonition <admonitions>` 。（大多数主题只有 "note" 和 "warning" 有特别的样式）

* Images:

  - :dudir:`image` （参考下面的 Images_ )
  - :dudir:`figure` (带有标题和可选例图的图)

* 额外的 body 元素：

  - :dudir:`contents <table-of-contents>` （一个本地的，即，只针对当前文档的一个目录表）
  - :dudir:`container` （一个可自定义类的容器，在 HTML 中生成一个外层的 ``<div>`` 很实用）
  - :dudir:`rubric` （与本节文档无关的一个栏目）
  - :dudir:`topic`, :dudir:`sidebar` （特别突出的 body 元素）
  - :dudir:`parsed-literal` （支持行内标记的字面块）
  - :dudir:`epigraph` （带有可选属性行的块引用）
  - :dudir:`highlights`, :dudir:`pull-quote` （带有自己的类属性的块引用）
  - :dudir:`compound <compound-paragraph>` （复合段落）

* 特殊表格：

  - :dudir:`table` （有标题的表格）
  - :dudir:`csv-table` （由逗号分隔值生成的表格）
  - :dudir:`list-table` （由列表的列表生成的表格）

* 特殊指令：

  - :dudir:`raw <raw-data-pass-through>` （包含未加工的目标格式的标记）
  - :dudir:`include` （包含另一个 reStructuredText 文档） -- 在 Sphinx 中，当给出一个要包含的文件的绝对路径时，这个指令将它作为相对于顶层源目录的路径处理。
  - :dudir:`class` （给下一个元素指定一个类属性） [1]_

* HTML 说明:

  - :dudir:`meta` （生成 HTML ``<meta>`` 标签）
  - :dudir:`title <metadata-document-title>` （覆盖文档标题）

* 影响标记：

  - :dudir:`default-role` （设定一个默认角色）
  - :dudir:`role` （创建一个新的角色）

  因为这些设定的范围仅仅是单个的文件，所以最好是用 Sphinx 的配置功能来设定 :confval:`default_role` 。

.. warning::

   *不要* 使用以下指令 :dudir:`sectnum` ， :dudir:`header` 和 :dudir:`footer` 。

由 Sphinx 添加的指令在 :doc:`directives` 中有说明。

基本上，一个指令包括名称，参数，可选项和内容等部分。（记住这些术语，在下一章介绍自定义指令时会用到）见如下例子::

   .. function:: foo(x)
                 foo(y, z)
      :module: some.module.name

      Return a line of text input from the user.

``function`` 是指令名称。这里有两个参数，分别是余下部分的第一行和第二行，以及一个可选项 ``module`` （如你所见，可选项由紧跟参数的行给出，且由冒号标明）。可选项必须和指令内容保持相同的缩进。

指令的内容在一个空行后开始且与指令开头保持相同的缩进。


图片
------

reST 支持一个 image 指令 (:dudir:`ref <image>`)，按如下方式使用::

   .. image:: gnu.png
      (options)

在 Sphinx 中使用时，文件名（这里是 ``gnu.png`` ）要么使用相对当前文件的相对路径，要么使用相对顶层源目录的绝对路径。
比如文件 ``sketch/spam.rst`` 可以用 ``../images/spam.png`` 和 ``/images/spam.png`` 来引用图片 ``images/spam.png`` 。

Sphinx 在构建目标文档时会自动将图片文件拷贝到输出目录相应的子文件夹中。（比如 HTML 格式输出的 ``_static`` 目录。）

图像大小的可选项（ ``width`` 和 ``height`` ）解释如下：
如果大小没有单位或单位是像素，仅在输出目标格式支持像素的情况下，可选项大小的值才有意义。
其他单位（比如表示 points 的 ``pt``）可以在生成 HTML 和 LaTeX 输出时使用，（后者将 ``pt`` 替换为 ``bp`` ，而这是 TeX 单位， ``72bp=1in`` ）。

Sphinx 以允许星号代表扩展名的方式扩展了标准 docutils 的行为::

   .. image:: gnu.*

Sphinx 在这种情况下会搜索所有符合给定模式的图像文件并判断其格式。
每一个构建器选择可选的最佳的图片， 如果给出的文件名是 ``gnu.*`` ，有两个文件
:file:`gnu.pdf` 和 :file:`gnu.png` 存在于源目录中， LaTeX 构建起将选择前者，而 HTML 构建起器则偏向于后者。
各构件器支持的图像类型以及优先级定义在 :ref:`builders` 中。

注意图像文件名不可以包含空格。

.. versionchanged:: 0.4
   添加支持以星号结尾的文件名。

.. versionchanged:: 0.6
   可以使用绝对路径来引用图像文件了。

.. versionchanged:: 1.5
   latex 目标格式支持像素单位了（默认为 ``96px=1in``）。


脚注
---------

脚注 (:duref:`ref <footnotes>`), 使用 ``[#name]_`` 来标记， 并在文档末尾的 "Footnotes" 栏目标题下添加脚注内容，像这样::

   Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

   .. rubric:: Footnotes

   .. [#f1] Text of the first footnote.
   .. [#f2] Text of the second footnote.

你也可以显式地指定脚注序号 (``[1]_``) 或使用没有名字的自动序号脚注 (``[#]_``) 。


引用
---------

Standard reST citations (:duref:`ref <citations>`) are supported, with the
additional feature that they are "global", i.e. all citations can be referenced
from all files.  Use them like so::

   Lorem ipsum [Ref]_ dolor sit amet.

   .. [Ref] Book or article reference, URL or whatever.

Citation usage is similar to footnote usage, but with a label that is not
numeric or begins with ``#``.


替换
-------------

reST supports "substitutions" (:duref:`ref <substitution-definitions>`), which
are pieces of text and/or markup referred to in the text by ``|name|``.  They
are defined like footnotes with explicit markup blocks, like this::

   .. |name| replace:: replacement *text*

or this::

   .. |caution| image:: warning.png
                :alt: Warning!

See the :duref:`reST reference for substitutions <substitution-definitions>`
for details.

.. index:: ! pair: global; substitutions

If you want to use some substitutions for all documents, put them into
:confval:`rst_prolog` or :confval:`rst_epilog` or put them into a separate file
and include it into all documents you want to use them in, using the
:rst:dir:`include` directive.  (Be sure to give the include file a file name
extension differing from that of other source files, to avoid Sphinx finding it
as a standalone document.)

Sphinx defines some default substitutions, see :ref:`default-substitutions`.


注释
--------

Every explicit markup block which isn't a valid markup construct (like the
footnotes above) is regarded as a comment (:duref:`ref <comments>`).  For
example::

   .. This is a comment.

You can indent text after a comment start to form multiline comments::

   ..
      This whole indented block
      is a comment.

      Still in the comment.


源编码
---------------

Since the easiest way to include special characters like em dashes or copyright
signs in reST is to directly write them as Unicode characters, one has to
specify an encoding.  Sphinx assumes source files to be encoded in UTF-8 by
default; you can change this with the :confval:`source_encoding` config value.


常见问题
-------

There are some problems one commonly runs into while authoring reST documents:

* **Separation of inline markup:** As said above, inline markup spans must be
  separated from the surrounding text by non-word characters, you have to use a
  backslash-escaped space to get around that.  See :duref:`the reference
  <substitution-definitions>` for the details.

* **No nested inline markup:** Something like ``*see :func:`foo`*`` is not
  possible.


.. rubric:: Footnotes

.. [1] When the default domain contains a :rst:dir:`class` directive, this
       directive will be shadowed.  Therefore, Sphinx re-exports it as
       :rst:dir:`rst-class`.
