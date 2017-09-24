# PEP 8 — Python 代码风格指南

>* auther : xjd
>* tags : python, coding-style
>* date : 2017-09-24

### 前言

由于英语水平的限制以及对Python语言的理解程度可能不足，在翻译时，可能会有词不达意甚至错译的情况，有些我自己无法理解的语句会将原文贴出，如果有更好的翻译，[Github传送门](xiejidong888@qq.com)。译文仅供参考。

![pep8](images/pep8.png)

### 简介

本文档给出了主要Python发行版中所包含的标准库的Python代码的编码约定。请参阅Python的C语言实现代码风格指南[1]

这份文档和 [PEP 257](https://www.python.org/dev/peps/pep-0257) (文档字符串的约定) 改编自Guido 的原始 Python 代码风格指南的文章，同时从 Barry 的风格指南[2] 中加入了一些额外的参考。

这份文档所描述的代码风格，随着时间的推进以及Python语言本身的改变，会引入新的编码约定，淘汰旧的过时编码约定。

许多项目拥有自己的编码约定，如果和此文档存在冲突，应该以项目的编码约定为主。

### 避免教条主义

Guido有一个重要见解是，代码被阅读的更多而不是被写的更多 (code is read much more often than it is written)，这里提供的准则旨在提高代码的可读性，并使其在广泛的Python代码中保持一致性，正如 [PEP 20](https://www.python.org/dev/peps/pep-0020) 说的:  "Readability counts".

编码风格指南的核心就是编码的一致性，这种一致性非常重要。模块或功能的一致性大于项目内的一致性。

然而，当不一致出现的时候，有时风格指南建议并不适用。有疑问的时候，用你的最佳判断。也可以参考看看其它的例子和决定，再选择最好的解决方案。同时毫不犹豫的说出你的疑惑！(去求助)

特别的：不要为了只是遵守代码风格指南而破坏了向后兼容性 (do not break backwards compatibility just to comply with this PEP!)。

一些需要随机应变的情况:

1. 当应用此代码风格会使得代码更加难以阅读时。即便对已经习惯这种风格(此文档)的人也是如此。
2. 与周围的代码保持一致(可能处于历史原因) - 尽管这是一个清理混乱代码的机会 (真正的 XP 风格)
3. 代码先于风格指南被提出，且没有其他理由需要对代码进行修改时。
4. 当代码需要兼容不支持样式指南中样式的旧版Python时。

### 代码的布局

#### 缩进

用4个空格表示一个缩进。

连续行应该用有隐式连接作用的圆括号、方括号、花括号包裹并且垂直对齐。或者使用悬挂缩进[^7]。当使用悬挂缩进时，应考虑以下事项：第一行不应有任何参数，应使用进一步缩进来清楚地将其区分为连续行。

***Yes:***

```python
# 与起始分割符垂直对齐
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# 缩进更多以和其他代码区分开。
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# 悬挂缩进需要多缩进一个层次。
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

***No:***

```python
# 当第一行包含参数时，只能和起始分隔符垂直对齐。
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# 当连续行无法和其他代码区分时，需要多进行缩进。
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

4个空格等于1缩进的规则在连续行中是可选的。

***Optional:***

```python
# 悬挂缩进中缩进不一定非得是4个空格。
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```

当条件判断语句(if-statement)足够长而需要进行多行书写时，需要注意 "if" 这个单词加一个空格，加一个括弧总共会占用4个空格从而导致和if的内嵌代码缩进一致产生视觉混淆。对于如何进一步区分这些条件行和内嵌代码，此文档没有明确规定，可以接受的情况包括但不仅限于以下：

```python
# 无额外缩进
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# 插入注释
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# 加入额外缩进
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

右圆括号/方括号/花括号在多行结构时，可以将其放在第一个非空字符下面，比如：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```

或者也可以放在最后一行行首：

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

#### 制表符还是空格？

空格是缩进的首选方法。

制表符仅用于和已使用制表符的代码保持一致性。

Python3 不允许制表符和空格混用。

Python2 中制表符和空格混用的代码应该统一转换为使用空格。

当使用带 '-t' 选项命令调用 Python2 解释器时，它会将制表符和空格混用视为警告，当使用 '-tt' 选项时，它会将其视为错误，这些选项被高度推荐使用。

#### 最大行长度

最大行长度限制在79个字符。

对于流动的大块少结构性限制的文本，行长度应该限制在72个字符。

限制所需的编辑器窗口宽度可以使多个文件并排打开，并且在使用代码审查工具阅读两个版本的代码时非常方便。

大部分工具中默认的行包装会破坏代码的可视化结构，使其更难被理解。这个限制是为了避免在编辑器可视化窗口宽度设定为80个字符时，最后一列可能被放置一些图形标记的情况，一些基于Web的工具可能不会提供动态的行包装功能。

有些团队需要更长的行长度。主要维护团队可以对这个问题达成一个约定，将其行长度从名义上的80个字符扩展到100个字符（除去最后一列用于给编辑器放置标记状态，实际长度是99个字符）。注释和文档行长度任然保持72个字符。

Python标准库是保守的，需要限制行为79个字符（文档字符串/评论72个字符）。

长行的首选方法是使用Python在圆括号、fa方括号和花括号中进行隐式的行延续。长的行可以在括号中进行换行。这些应该优先用于使用反斜杠进行行延续。

反斜杠在某些情况下任然适用，比如，在长with-statements中，无法隐式的进行行延续，这是需要使用反斜杠：

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

（类似if-statements，这里的with语句延续行的缩进和with内嵌代码同样产生了缩进混淆问题）

另外一个例子是 assert statements。

确保适当的对延续行进行缩进。

#### 应该在二元运算符之前断行还是之后断行？

几十年来，推荐的方式都是在二元运算符后面断行，这对代码可读性会产生两个坏影响：一是运算符分散在屏幕上不同列，二是运算符被移到了它运算元的之前一行，阅读时需要耗费额外的精力来区分是对这个运算元做加法还是减法：

```python
# No: operators sit far away from their operands
income = (gross_wages +
          taxable_interest +
          (dividends - qualified_dividends) -
          ira_deduction -
          student_loan_interest)
```

为了解决可读性问题，数学家和出版社们采取了截然相反的方式，Donald Knuth在他的《Computers and Typesetting》系列中对传统的规则解释道：“Although formulas within a paragraph always break after binary operations and relations, displayed formulas always break before binary operations”[^3]。

```python
# Yes: easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

在Python代码中，不管在二元运算符之前还是之后断行都是允许的，只要保持一致即可。新代码应听从 Knuth 的风格建议。

#### 空行

顶层的函数和类定义前应有两空行。

类中的方法定义前应只有一行空行。

额外的空行（少量的）可以用来分离功能相关的一组函数。Blank lines may be omitted between a bunch of related one-liners (e.g. a set of dummy implementations).

利用空行来表明函数中的逻辑结构。

Python允许 control-L 形式作为空白字符，许多工具将这些字符当成分页符，所以你也可以用其来对你的文件进行分页。注意，有些基于Web的代码审查工具可能无法识别这个字符形式而将其显示为其他标记。

#### 源文件编码

Python的核心发行版代码应始终使用 UTF-8 进行编码（Python2 中用 ASCII）。

Python2中使用ASCII码编码，和Python3中使用UTF-8编码时，**不应该**进行编码声明。

在标准库中，非默认编码应只用于测试目的或者当注释和文档字符串需要用到非ASCII码字符是，否则，推荐用 `\x, \u, \U, \N` 来转义非ASCII字符。

对于Python3.0及之后的版本，标准库需要遵循一些约定：所有Python标准库中的标识符**必须**只使用利用ASCII码字符，**应该**尽可能的使用英文字符（许多情况下，简写和专业短语并不是使用的英文字符）。同时，字面字符串和注释必须使用ASCII码字符。唯一例外的情况是，(a) 测试非ASCII编码特性，(b) 作者姓名，如果其姓名中包含非拉丁字符，**必须**提供翻译的拉丁版本。

国际性的开源项目鼓励其采用相似的策略。

#### 导入库

* 导入时每个库都应该分行

  ```python
  Yes: import os
       import sys

  No:  import sys, os
  ```

  这种情况是可以的：

  ```python
  from subprocess import Popen, PIPE
  ```

* 导入语句应该放在文件的最上面，同时处于模块注释和文档注释之下，模块全局变量和常量之上。

  导入顺序应该是：

  1. 标准库
  2. 相关第三方库
  3. 本地应用或库

  在每一类导入之间加一个空行。

* 推荐使用绝对方式导入，一般而言绝对方式导入更易读，当导入系统配置错误时表现的更好（至少可以给出更好的错误信息）。

  ```python
  import mypkg.sibling
  from mypkg import sibling
  from mypkg.sibling import example
  ```

  然而，相对导入仍然是绝对导入的可接受替代方案，尤其是当导入拥有复杂结构的包时，使用绝对导入会增加不必要的冗余。

  ```python
  from . import sibling
  from .sibling import example
  ```

  标准库代码应该避免复杂的包结构同时使用绝对导入方式。

  隐式相对导入已经从Python3中移除。

* 从包含类的模块中导入类的时候，通常可以这些写：

  ```python
  from myclass import MyClass
  from foo.bar.yourclass import YourClass
  ```

  如果和本地名称产生了冲突，可以这样写：

  ```python
  import myclass
  import foo.bar.yourclass
  ```

  然后用 `myclass.MyClass` 和 `foo.bar.yourclass.YourClass` 来调用。

* 避免使用通配符导入（`from <module> import *`），因为不清楚命名空间中存在哪些名称，所以会使读者和许多自动化工具感到困惑。但是有一种情况需要用到通配符导入，就是将内部接口重新发布为公共接口

  当使用这种方式重新发布接口时，下面的指南对于公共和内部接口仍然适用。

#### 模块层次的魔法名字

模块级“dunders”（即名字前后有两个下划线）如`__all__，__author__，__version__`等等，除了`__future__`都应该放在模块注释之后和导入语句之前。Python中future导入语句必须所有语句之前出现，除了文档注释。

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

### 字符串引号

在Python中，单引号和双引号的作用是一样的，此文档不会选择偏好哪一个，你只需要选择其中一个并坚持就行了。当需要在字符串中使用引号时，用另外一种引号来包围字符串可以避免使用反斜杠，提高代码易读性。

对于三引号，始终用双引号表示以确保一致性 [PEP 257](https://www.python.org/dev/peps/pep-0257)。

### 表达式和语句中的空格

#### 难念的经

在下面几种情况下避免使用无关的空格。

* 立即在圆括号、方括号、花括号前后使用空格。

  ```python
  # Yes: 
  spam(ham[1], {eggs: 2})

  # No:  
  spam( ham[ 1 ], { eggs: 2 } )
  ```

* 在一个跟着右括号的逗号后面。

  ```python
  # Yes: 
  foo = (0,)

  # No:  
  bar = (0, )
  ```

* 在逗号、分号、冒号之前。

  ```python
  # Yes: 
  if x == 4: print x, y; x, y = y, x

  # No:  
  if x == 4 : print x , y ; x , y = y , x
  ```

* 在切片中，冒号就像一个二进制运算符，在两边应该有相等量的数（把他当成优先级最低的运算符），在一个扩展切片中，所有冒号的两边的空格数必须相同。一个另外：如果切片中有一个参数省略了，那空格也将被省略。

  ```python
  # Yes:

  ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
  ham[lower:upper], ham[lower:upper:], ham[lower::step]
  ham[lower+offset : upper+offset]
  ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
  ham[lower + offset : upper + offset]

  # No:

  ham[lower + offset:upper + offset]
  ham[1: 9], ham[1 :9], ham[1:9 :3]
  ham[lower : : upper]
  ham[ : upper]
  ```

* 在函数的参数列表的左括号前。

  ```python
  # Yes: 
  spam(1)

  # No:  
  spam (1)
  ```

* 在切片的左括号前。

  ```python
  # Yes: 
  dct['key'] = lst[index]

  # No:  
  dct ['key'] = lst [index]
  ```

* 为了对齐等号而在之前添加多个空格。

  ```python
  # Yes:

  x = 1
  y = 2
  long_variable = 3

  # No:

  x             = 1
  y             = 2
  long_variable = 3
  ```

#### 其他建议

* 避免在任何地方尾随一个空格，这个空格通常是不可见的，它会引发疑惑：比如，一个反斜杠后面跟一个空格时，是否算作一个续行标记。有些编辑器不会允许这种行为，大部分项目里面也会有预提交来放止这种情况发生。

* 总是在二元运算符两边加上空格：等号（=）、增强赋值（+=、-=等）、比较（==、>、<、!=、<>、<=、>=、in、not in、is、is not）、布尔运算（and、or、not）。

* 如果表达式中有不同优先级的运算符，可以在最低优先级运算符两边加上空格，但是不要超过一个且运算符两边空格数相等。

  ```python
  # Yes:

  i = i + 1
  submitted += 1
  x = x*2 - 1
  hypot2 = x*x + y*y
  c = (a+b) * (a-b)

  # No:

  i=i+1
  submitted +=1
  x = x * 2 - 1
  hypot2 = x * x + y * y
  c = (a + b) * (a - b)
  ```

* 在关键字参数的等号两边，不要加空格。

  ```python
  # Yes:

  def complex(real, imag=0.0):
      return magic(r=real, i=imag)
      
  # No:

  def complex(real, imag = 0.0):
      return magic(r = real, i = imag)
  ```

* 函数注解中，对于冒号应该采用通常的办法来添加空格，而对于`'->'`应该在两边都加上空格（查看更多关于函数注解[Function Annotations](https://www.python.org/dev/peps/pep-0008/#function-annotations)）。

  ```python
  # Yes:

  def munge(input: AnyStr): ...
  def munge() -> AnyStr: ...

  # No:

  def munge(input:AnyStr): ...
  def munge()->PosInt: ...
  ```

* 当参数注释时，如果有默认值，则在等号两边加上空格。

  ```python
  # Yes:

  def munge(sep: AnyStr = None): ...
  def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...

  # No:

  def munge(input: AnyStr=None): ...
  def munge(input: AnyStr, limit = 1000): ...
  ```

* 复合语句通常是不被鼓励的（多条语句写在同一行）

  ```python
  # Yes:

  if foo == 'blah':
      do_blah_thing()
  do_one()
  do_two()
  do_three()

  # Rather not:

  if foo == 'blah': do_blah_thing()
  do_one(); do_two(); do_three()
  ```

* 但是有时将很短的 if/for/while 放在同一行是可以的。但是永远别将有多个关键字的语句（multi-clause statements）放在同一行，不要试图折叠它们。

  ```python
  # Rather not:
  if foo == 'blah': do_blah_thing()
  for x in lst: total += x
  while t < 10: t = delay()

  # Definitely not:
  if foo == 'blah': do_blah_thing()
  else: do_non_blah_thing()

  try: something()
  finally: cleanup()

  do_one(); do_two(); do_three(long, argument,
                               list, like, this)

  if foo == 'blah': one(); two(); three()
  ```

### 什么时候尾随一个逗号

尾随一个逗号通常是可选的，除非是为了得到一个元素的元组而强制添加（在Python2的`print`语句中是有意义的）。为清楚起见，建议在后者加上括号（技术上是冗余的）

```python
# Yes:
FILES = ('setup.cfg',)

# OK, but confusing:
FILES = 'setup.cfg',
```

当使用版本控制系统时，当需要扩展一串参数时，多尾随一个逗号通常是有用的。这种模式是将每一个参数放在单独的一行，然后在行尾添加一个逗号，再在最后，另起一行括上右括号。如果将一串参数写在同一行，多添加一个逗号通常是无用的（除了单元素元组情况外）。

```python
# Yes:
FILES = [
    'setup.cfg',
    'tox.ini',
    ]
initialize(FILES,
           error=True,
           )
           
# No:
FILES = ['setup.cfg', 'tox.ini',]
initialize(FILES, error=True,)
```

### 注释

与代码相悖的注释比没有注释更加糟糕，始终将更新代码注释保持最高的优先级。

注释应该是完整的句子。如果注释是一个短语或句子，它的第一个词应该大写，除非它是一个以小写字母开头的标识符（不要改变标识符的写法）！

如果注释很短，则何时结束注释可以省略。块注释一般由完整句构成的一个或多个段落组成，每个句子应以一个句号结束。

在句子结束后跟两个空格。[注：这一条好像被严重鄙视了。]

用英语写作时，遵循Strunk and White原则。

来自非英语国家的Python程序员，请务必用英文来对你的代码进行注释，除非你120%确定你的代码不会被不懂你的语言的程序员读到。

#### 块注释

块注释一般写在被注释代码的上方，且和被注释代码保持相同的缩进，每一行注释都以一个'#'和一个空格开头。

块注释中用单独的'#'进行断行。

#### 内联注释(行尾注释)

尽量少用内联注释。内联注释是与语句在同一行上的注释。内联注释应该和代码保持至少两个空格的间隔。他们应该以一个'#'和一个空格开头。

不要进行无必要的注释来分散注意力：

```python
x = x + 1                 # Increment x
```

但是如果这样注释是可以的：

```python
x = x + 1                 # Compensate for border
```

#### 文档注释

关于良好书写文档注释的约定文件（又名 “docstrings”）已经作为[PEP 257](https://www.python.org/dev/peps/pep-0257)永久生效。

* 对所有公开的模块、函数、类、方法进行文档注释，对于非公开的则没有必要，但是最好说明代码时干什么的。文档注释应该放在 `def`下面。

* [PEP 257](https://www.python.org/dev/peps/pep-0257)描述了良好的文档注释规范。注意最重要的一点：用 `"""` 结束文档注释时，应该单独起一行。

  ```python
  """Return a foobang

  Optional plotz says to frobnicate the bizbaz first.
  """
  ```

* 对于单行文档注释，应该写在同一行。

### 命名约定

Python库的命名规范有些混乱，所以我们没有强制要求保持一致性。尽管如此，我们仍然推荐目前的命名标准。新的模块和包（包括第三方框架）应该遵循这些标准，但是现有的不同风格的库，保持内部一致性是首选。

#### 最重要的原则

作为对用户可见的公共API的命名，应该以反映其用处为主，而非反映实现方法。

#### 命名样式分类

有许多种不同的命名样式， It helps to be able to recognize what naming style is being used, independently from what they are used for。

以下样式通常是可区分的：

* `b` (单个小写字母)

* `B` (单个大写字母)

* `lowercase`

* `lower_case_with_underscores`

* `UPPERCASE`

* `UPPER_CASE_WITH_UNDERSCORES`

* `CapitalizedWords` (or CapWords, or CamelWords — 这么命名主要因为其大写字母看起来像驼峰[4])。有时也叫 StudlyCaps。

  注意：当在 CapWords 中使用缩略词时，大写缩略词中的所有字母。比如 `HTTPServerError` 优于`HttpServerError`。

* `mixedCase` (和 CapWords 不同的是首个字母全部小写)

* `Capitalized_Words_With_Underscores` (丑！)

还有一种使用简短唯一前缀的方法来标记同一组名字，在Python中这种方法不是很常用。为了完整性，也会提到使用这种方法的情况。比如，函数`os.stat()`返回一个拥有传统命名的元组，`st_mode, st_size, st_mtime`等等。这样做是为了强调和 POSIX的调用结构保持对应，以方便程序员。

此外，在命名前后添加下划线的特殊情况也是可以使用的（这种方式通常可以和任何命名规则联合使用）。

* `_single_leading_underscore` : 表示弱内部使用，比如，`from M import *` 不会导入以下划线开头的对象。
* `single_trailing_underscore_` : 在后面加下划线是为了不和 Python 中内建的关键字冲突。比如，`Tkinter.Toplevel(master, class_='ClassName')`。
* `__double_leading_underscore` : 当命名一个类属性时，方便带上前缀（在类 `Foobar` 内部，`__boo` 变成 `_Foobar__boo`）。
* `__double_leading_and_trailing_underscore__` : 魔法对象或者用户自定义的属性。比如，`__init__, __import__, __file__`。不要自定义这类名字，最好使用文档里面已经标注的。

#### Prescriptive: 命名约定

##### 禁忌

不要将字符“l”（小写字母，音同'el'）、“O”（大写字母，音同'oh'）或“I”（大写字母，音同'eye'）作为单个字符变量名。

在某些字体中，这些字母和数字1和0无法区分，如果非要用'el'，最好用大写L。

##### ASCII兼容性

标准库中使用的标识符必须是ASCII兼容的，如PEP 3131的策略部分中描述的那样。

##### 包和模块命名

模块应该有简短的小写字母。如果可以提高可读性，则可以在模块名中使用下划线。Python包也应该有简短的全部小写的名称，不鼓励使用下划线。

当一个扩展模块使用C/C++完成并有一个更高层次的Python模块作为接口时，在C/C++模块的名字前加一个下划线（比如，`_socket`）。

##### 类名

类名应该使用驼峰命名法。

当接口已记录在文档主要作为可调用对象时，可以使用函数的命名规范。(The naming convention for functions may be used instead in cases where the interface is documented and used primarily as a callable. )

注意，对内置的名字单独约定：大多数的内置对象的名字是单个单词（或两个词写在一起），包括异常类命名和内置常量命名都使用驼峰命名法。

##### 类型变量名

在[PEP 484](https://www.python.org/dev/peps/pep-0484) 中介绍的类型变量名一般使用简短的驼峰命名法：`T, AnyStr, Num`等，最好在名字前加上前缀，比如`_co, _contra`等来表明其行为特点。

```python
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

##### 异常名

因为异常应该是类，所以类命名约定适用于这里。但是，您应该在异常名称上使用后缀“Error”（如果该异常确实表示的是一个错误）。

##### 全局变量名

（假设这些变量都是在这些在同一个模块中使用的）这些约定与函数的约定大致相同。

通过`from M import *`来导入的模块，应该用`__all__`来防止全局变量的导入。或者用一个老方法，在变量名前加上一个下划线以表明这些变量是非公开的。

##### 函数名

函数名应该全部小写，用下划线来分割单词以提高可读性。

混合风格只允许在为了保持向后兼容的已经是混合风格的上下文（比如，treading.py）中使用。

##### 函数和方法的参数

总是使用`self`作为实例方法的第一个参数。

总是使用`cls`作为类方法的第一个参数。

如果函数参数的名称与保留关键字冲突，一般最好追加一个尾部下划线，而不是使用缩写或拼写损坏。因此`class_`比`clss`更好。（最好是使用同义词避免这种冲突。）

##### 方法名和实例变量

使用函数命名规则：小写，用下划线分隔单词，以提高可读性。

仅在非公开的方法和非公开的实例变量名前面添加一个下划线。

To avoid name clashes with subclasses, use two leading underscores to invoke Python's name mangling rules.（为了避免与子类名冲突，援引python的名字转换规则，添加两个前导下划线）

Python会将名字和类名连接在一起，如果类`Foo`有一个属性`__a`，这个属性无法被`Foo.__a`访问（但是仍然可以被`Foo._Foo__a`访问），一般来说，双下划线应该只是用在为了避免类中属性为子类时名字冲突的情况。

注：有关于`__names`使用一些争议（见下文）。

##### 常量

常量通常定义在模块级别，所有字母全部大写，并用下划线分隔单词，比如`MAX_OVERFLOW, TOTAL`。

##### 继承的设计

始终优先确定一个类的方法或者实例中的变量（统称为属性）是公开的还是非公开的。如果心存疑虑，那就选择非公开的，因为把非公开的改为公开的总比公开的改为非公开的容易。

公开的类属性是给那些和该类没什么关系的对象使用的，同时承诺避免向后不兼容的更改。非公开的属性是不打算被第三方使用的，因为无法保证这些非公开属性不被更改甚至删除。

我们不使用“私有”一词，因为在Python中并没有什么属性是真正私有的（减少一些不必要的工作量）。

另一类属性是“子类API”的一部分（通常在其他语言称为`protected`）。有些类是为了继承、扩展或修改类的行为方面而设计得。在设计这样一个类时，要谨慎地决定哪些属性是公开的，哪些属性是只供基类使用的子类API的一部分。

所以，以下是让代码Pythonic的指导方针：

* 公开的属性不应该有前导下划线。
* 如果你的公共属性的名称与保留关键字冲突，在属性名尾部附加一个下划线。这比缩写或故意错误的拼写更好。（然而，尽管有这条规则，`cls`是一个众所周知的用来表示类的一个变量名或参数名，尤其是类方法的第一个参数。）
  注：类方法的参数命名推荐规则见上文
* 对于简单的公开数据属性，最好不要用复杂的存和取的方法来管理，而是简单的将属性名暴露即可。Python提供了简单的扩展方法，如果你发现你的数据属性需要扩展成一个拥有复杂行为的方法时，用函数式属性来隐藏掉方法机制，从而可以使用属性的方法来访问它。
  注1：函数式属性仅可以在新式类中使用
  注2：尽量不要让函数行为产生副作用，但是像缓存这种事可以的。
  注3：尽量不要讲函数式属性用在计算量巨大的函数上，因为这会让访问它的人觉得属性访问的代价非常小。
* 如果你的类有可能被继承（If your class is intended to be subclassed），但是你又不想让你这个类的属性被子类所用，可以考虑在属性名前加两个下划线，而尾部无下划线，这会触发Python的名字变换算法（name mangling algorithm），类名将会拼接到属性名的前面，这可以避免子类命名属性的时候和基类属性名重名的问题。
  注1：注意名字变换只是简单的把类名拼接到属性名前，如果你的属性名前本来就有类名，那么仍然有可能冲突。
  注2：名字转换有一些特定用途，比如调试和`__getattr__()`，虽然不方便，但是名字转换有良好的文档以及易于手动操作。
  注3：不是每个人都喜欢名字转换，Try to balance the need to avoid accidental name clashes with potential use by advanced callers.

#### 公共接口和内部接口

任何向后兼容性保证仅适用于公共接口。因此，用户能够清楚地区分公共接口和内部接口是非常重要的。

文档注释过的接口被认为是公开的，除非文档明确声明它们是临时或内部接口，不受通常的向后兼容性保证。所有未记录的接口都应该是内部的。

为了更好地支持自省机制，模块应该显式在`__all__`属性中声明自己的公共接口。`__all__`设置为一个空列表表示模块没有公共API。

就算`__all__`已经设置妥当，内部接口（包名，模块名，类名，函数名，属性名或者其他名字）仍然应该加上一个前导下划线。

如果命名空间（包、模块、类）是内部的，那么其包含的接口也被认为是内部的。（An interface is also considered internal if any containing namespace (package, module or class) is considered internal.）

导入的名字应该被认为是一种细节实现的方式，其他模块不应该依赖于对这些名字的简介访问，除非他们是模块中拥有明确文档的API，比如 `os.path` 或者包的 `__init__`模块将子模块的功能暴露出来。

### 编程建议

1. 代码不应该选择一种在其他Python（PyPy、Jython、IronPython、Cython、Psyco等等）中不被鼓励的实现。
   比如，在以“in-place”的方式连接字符串时，不要依赖于在CPython中有效的实现形式：`a += b or a = a + b` ，就算在CPython中，这种形式也是非常脆弱的（它只适用于某些类型），而且根本不存在不使用引用计数的实现。在库中性能敏感的部分，应该使用 `''.join()` 的形式，这样就能保证在其他Python中也是线性运行时间。

2. 在比较单例实例时，应该使用 `is` 或 `is not` 而不是相等号。
   另外，注意当你的意思是 `if x is not None` 时，才可以写成 `if x`，比如在测试一个变量或者参数是否是None还是其他值时。其他值有可能是有类型的（比如容器之类），这样有可能在布尔测试时判断为假。

3. 用 `is not` 而不是 `not ... is` 虽然两者的意思是一样的。

   ```python
   # Yes:
   if foo is not None:

   # No:
   if not foo is None:
   ```

4. 当实现比较丰富的比较操作时，最好实现所有六种比较操作（`__eq__, __ne__, __lt__, __le__, __gt__, __ge__`）而不是只是实现其中一种操作。
   `functools.total_ordering()` 装饰器可以补全缺省的比较操作。

5. [PEP 207](https://www.python.org/dev/peps/pep-0207) 表明，自反性规则在Python中是成立的，所以，解释器会将 `y > x` 变换成 `x < y`，`y >= x` 变换成 `x <= y`，`x == y` 变换成 `x != y`。`sort()` 和 `min()` 使用 `<` 操作符，`max()` 使用的是 `>` 操作符，但是，最好还是实现所有六种操作以避免在其他上下文中产生歧义。

6. 总是使用 `def` 语句，而不是直接将lambda表达式绑定到标识符。 

   ```python
   # Yes:
   def f(x): return 2*x

   # No:
   f = lambda x: 2*x
   ```

   第一个表单意味着生成的函数对象的名称是 `f` 而不是泛型“< lambda >”。这在一般的回溯和字符串表示更有用。赋值语句的使用消除了lambda表达式在和显式 `def` 语句（即它可以嵌入较大表达式中）比较时所能提供的唯一好处。

7. 从 `Exception` 而不是 `BaseException` 来继承异常类，直接从 `BaseException` 继承的异常类通常是用来表示不要试图捕捉该异常。
   基于代码所捕获的异常的区别来设计异常层次结构，而不是引发异常的位置。要回答的是“什么东西出错了？”，而不是只是说“出错了”。（详见 [PEP 3151](https://www.python.org/dev/peps/pep-3151) 来了解内建异常层次结构）
   类名约定可以应用在此处，如果你的异常类表示的是一个错误，那应该在类名后面添加“Error”后缀，用于非本地流控制或其他形式信令的非错误异常不需要特殊后缀。

8. 适当的使用异常链。在Python3中，`raise x from y` 应该清晰的表明这种替换而不失去原始的回溯信息。
   当替换一个内部异常时（Python2中用 `raise x` ，Python3.3+中用 `raise x from None`），确保相关详细信息都被转移到了新的异常中（比如当 `KeyError`转换为 `AttributeError` 时保存属性名信息，或者将原始异常信息的文本嵌入到新异常中）。

9. 在Python2中，当抛出一个异常时，使用 `raise ValueError('message')` 而不是老的表达方式 `raise ValueError, 'message'` 。
   后一种表达方式在Python3中不合法。因为括号的使用，也意味着当异常参数过长或者包含字符串时，你不需要使用续行符了。

10. 当捕捉异常时，尽可能的提及具体的异常，而不是使用一个简单的 `except:` 语句。比如用以下形式：

   ```python
   try:
       import platform_specific_module
   except ImportError:
       platform_specific_module = None
   ```

   毫无修饰的使用 `except:` 会捕获 `SystemExit` 和 `KeyboardInterrupt` 异常，使得程序更难以被 Control-C 中断，也会掩盖其他问题。如果你想捕获用来表示程序错误的所有异常，使用 `except Exception:` ，毫无修饰的 `except:` 等于 `except BaseException` 。

   一个很好的经验是限制在以下两种情况中使用 `except:`

   * 如果异常处理程序将打印或记录异常回溯信息，至少让用户知道发生了一个错误。
   * 如果代码在遇到错误后需要做一些善后工作，然后再让异常向上传播，使用 `try ... finaly` 可以更好的处理这种情况。

11. 当捕获操作系统异常时，参考Python3.3中`errno`模块中的异常结构。

12. 另外，对于所有 `try...except...` 语句，将 `try` 中的必要代码最少化，这样能避免掩盖错误。

    ```python
    # Yes:
    try:
        value = collection[key]
    except KeyError:
        return key_not_found(key)
    else:
        return handle_value(value)
        
    # No:
    try:
        # Too broad!
        return handle_value(collection[key])
    except KeyError:
        # Will also catch KeyError raised by handle_value()
        return key_not_found(key)
    ```

13. 当某个资源仅在一段代码中起作用时，用 `with` 语句来及时和有效的关闭/清理该资源。使用 `try...finally...`也是可以接受的。

14. 无论何时获取和释放资源，都应该通过单独的函数或方法调用上下文管理器。例如:

    ```python
    # Yes:
    with conn.begin_transaction():
        do_stuff_in_transaction(conn)
        
    # No:
    with conn:
        do_stuff_in_transaction(conn)
    ```

    后一个例子，除了能在 transaction 之后关闭连接之外，没有提供任何信息表明 `__enter__` 和 `__exit__` 方法做了些事情。明确表示出来是很重要的。

15. 保持返回语句的一致性，在一个函数中，要不所有 return 都能返回一个表达式，要不就都不要。如果有一些语句返回了表达式，所有没有返回值的 return语句应该明确的写成 `return None` 。并且如果函数能运行到末尾，那应该在末尾明确的写一个返回语句。

    ```Python
    # Yes:
    def foo(x):
        if x >= 0:
            return math.sqrt(x)
        else:
            return None

    def bar(x):
        if x < 0:
            return None
        return math.sqrt(x)
      
    # No:
    def foo(x):
        if x >= 0:
            return math.sqrt(x)

    def bar(x):
        if x < 0:
            return
        return math.sqrt(x)
    ```

16. 使用字符串方法而不是字符串模块。
    字符串方法总是更快，而且和 unicode 字符串共享API。在Python2.0以前的版本中因为兼容问题忽略此规则。

17. 使用 `''.startswith()` 和 `''.endswith()` 代替字符串切片来检查字符串前缀和后缀，这两个方法看上去更加干净且不容易出错：

    ```python
    # Yes: 
    if foo.startswith('bar'):
      
    # No:  
    if foo[:3] == 'bar':
    ```

18. 对象类型比较应该用 `isinstance()` 方法而不是直接比较类型。

    ```python
    # Yes: 
    if isinstance(obj, int):

    # No:  
    if type(obj) is type(1):
    ```

    当检查对象是否是字符串时，请记住它可能也是一个unicode字符串！在Python 2，str和unicode字符都有一个共同的基类，basestring，所以你能这样做：

    ```Python
    if isinstance(obj, basestring):
    ```

    在Python 3，unicode和basestring不再存在（只有str），bytes object 不再是一种字符串（它是一个整数序列代替）

19. 对于序列，（string, lists, tuples），记住，空序列的条件判断为假。

    ```python
    # Yes: 
    if not seq:
         if seq:

    # No: 
    if len(seq):
        if not len(seq):
    ```

20. 不要写依赖于有意义的尾空格的字符串，这些尾空格视觉上难以区分，而且一些编辑程序（比如最近的 reindent.py）会修剪掉这些空格。

21. 不要用`==`来比较布尔值。

    ```python
    Yes:   if greeting:
    No:    if greeting == True:
    Worse: if greeting is True:
    ```

#### 函数注释

随着 [PEP 484](https://www.python.org/dev/peps/pep-0484) 的接受，函数注释的样式规则正在改变。

* 为了向前兼容，Python 3代码中的函数注释最好使用 [PEP 484](https://www.python.org/dev/peps/pep-0484)语法。（在前一节中有一些注释的格式化建议。）

* 先前在PEP中推荐的注释样式的实验不再受到鼓励。

* 然而，在标准库`stdlib`之外，PEP 484规定的样式试验正在被鼓励。例如，在大型的第三方库或PEP 484风格的注释类型的应用中，浏览它的代码是多么容易添加注释，并观察是否他们的存在增加了代码的可理解性。

* Python 的标准库应该保守的接受这种注释风格，但是新代码和大型重构是允许使用这种注释风格的。

* 如果想要让注释发挥一些不一样得作用，建议使用这种格式：

  ```Python
  # type: ignore
  ```

  在文件的顶部；这告诉类型检查器忽略所有注释。（在 [PEP 484](https://www.python.org/dev/peps/pep-0484) 中可以找到更细粒度的禁用类型检查器的方法。）

* 像linters一样，类型检查器是可选的，单独的工具。默认情况下，Python解释器不应该因为类型检查而发出任何消息，也不应该基于注释改变它们的行为。

* 不想使用类型检查的用户可以忽略他们，然而，第三方库包的用户可能希望在这些包上运行类型检查器。为此[PEP 484](https://www.python.org/dev/peps/pep-0484)建议使用桩文件：.pyi 文件会优先于对应的.py文件被读取，桩文件可以被分配到一个库中，或者使用 Typeshed repo[5]来分别存放（在作者的许可下）。

* 对于需要向后兼容的代码，可以以注释的形式添加类型注释。参见 [PEP 484](https://www.python.org/dev/peps/pep-0484) 的相关章节[6]。



### 参考文献

> [1]. [PEP 7](https://www.python.org/dev/peps/pep-0007), Style Guide for C Code, van Rossum
>
> [2]. Barry's GNU Mailman style guide <http://barry.warsaw.us/software/STYLEGUIDE.txt>
>
> [3]. Donald Knuth's *The TeXBook*, pages 195 and 196.
>
> [4]. http://www.wikipedia.com/wiki/CamelCase>
>
> [5]. Typeshed repo <https://github.com/python/typeshed>
>
> [6]. Suggested syntax for Python 2.7 and straddling code <https://www.python.org/dev/peps/pep-0484/#suggested-syntax-for-python-2-7-and-straddling-code>

[^7]: *Hanging indentation* is a type-setting style where all the lines in a paragraph are indented except the first line. In the context of Python, the term is used to describe a style where the opening parenthesis of a parenthesized statement is the last non-whitespace character of the line, with subsequent lines being indented until the closing parenthesis.