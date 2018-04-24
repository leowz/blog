---
title: RegularExpression.md
date: 2016-11-02 22:24:45
tags:
---
`Nov 2 2016`

# 正则表达式RegexOne译文(Toutorial From RegexOne)
[TOC]

# 第一课 介绍、ABC、123    

正则表达式在从文本如代码、日志文件、电子表格、或文件中抽取信息是非常有用的。虽然这个语言背后有很深入的理论背景，在接下来的课程和练习中我们将探讨正则表达式的实践用途，让读者可以快速上手使用。

在用正则表达式之前先要明确的是，在正则表达式中所有东西都用字符表达，我们写的是一些模式能够匹配一些特定的字符串。大部分模式用ASCII，其中使用了字母，数字，表达和你在键盘上能找到的字符，但是可以用Unicode字符来匹配任何语言的文本。
<!--more-->
以下是一个小例子：
Match   abcdefg 
Match   abcde 
Match   abc

pattern:
> `abd`

字符如果去查ASCII码的话，包括字母、数字和其他符号。
在接下来的课中我们会学习一下元字符用来匹配特定的字符。

- \d :匹配0-9的任意数字字符
- \D :匹配任意飞数字的字符

`\`符使其与普通d／D 字母区分开，表示其是一个元字符。 

例子：    
Match   abc123xyz   
Match   define "123"   
Match   var g = 123;

> `123`; `\d`; `\D` ; `\d\d\d` 等等

# 第二课 点 .

在正则表达式中，经常会遇见的情况是，你想匹配格式一样内容未知的文本，比如电话号码、邮政编码。

句点`.`元字符可实现，其匹配任意一个单个字符。

句点元字符复写了句号字符的功能，如果想匹配句号而不是枇杷任意一个字符使用`\.`

例子：

Match   cat.
Match   896.
Match   ?=+.
Skip    abc1

> `...\.`; `\.`

# 第三课 匹配指定字符集 []

句点操作符功能很强大，但匹配太广泛，如果我们匹配电话号码，我们并不期望匹配到字母。

在方括号`[ ]`中指明具体的字符，可以实现对具体一个字符的条件匹配。比如，`[abc]`只会匹配a，b或c。

例子：
Match   can 
Match   man
Match   fan
Skip    dan
Skip    ran
Skip    pan

> `[cmf]an`; 或 `[^drp]an`；

# 第四课 不匹配指定字符集 [^]

有些情况下，我们不需要匹配某一些字符。比如，电话号码匹配中，也许我们不想匹配650地区的号码

实现不匹配指定字符集功能，我们用`[^abc]`这样这个字符就不匹配a、b、c。

例子：
Match   hog
Match   dog
Skip    bog 

> `[^b]og`; 或 `[hd]og`

# 第五课 字符范围 [ - ]

有些情况下，我们要匹配的字符在一些字符中或在某些字符范围中，我们用`-`连接两个字符表示范围。

比如`[0-6]`表示0到6，`[^n-p]`表示匹配一个不是从n到p之内的字符

在一个方括号里也可以用多个范围。比如`[A-Za-z0-9]`是用了匹配一个字母或数字的表达式。
`\w` 原字符表示一个字母或数字的字符(AlphaNumeric) 
`\W` 表示一个非字母数字的字符(Non-AlphaNumeric)

例子：
Match   Ana
Match   Bob
Match   Cpc
Skip    aax
Skip    bby 
Skip    ccz

> `[A-C][n-p][a-c]`

# 第六课 重复内容 {}

如果我们想匹配重复的字符，怎么办呢？一个方法是重复的写出匹配表达式，如`\d\d\d`这样可以匹配3个数字。
一种更方便的方法是使用大括号注释。

`a{3}` 表示匹配a这个字符3次。有些引擎还能实现如`a{1,3}`表示把a匹配不少于1次不多余3次。
这个限定符可以用在任何字符上。比如`[wxy]{5}` 匹配5次w、x、y任意一个的字符。`.{2,6}`匹配2到6次任意字符。

例子：

Match   wazzzzzup
Match   wazzzup 
Skip    wazup

> `waz{3,5}up`

# 第七课   * 与 + 

正则表达式中的一个强大的功能是匹配任意数量的字符。例如， 假设让你写一个捐款表格，其中一个捐款数量字段需要填入一个单位是美元的数字。有钱的捐款者可能卷25,000$而一般人则卷25$。

一种解决这样匹配的方式用到称为克莱尼(斯蒂芬·科尔·克莱尼,美国数学家、逻辑学家、他也是正则表示法的发明者)+和克莱尼*的概念。其表示0个或更多和一个或更多。具体的说，一个上面捐款的匹配可以是：
`\d*`表示一个数字字符或更多。
一个更好的写法则为：
`\d+`表示一个数字或更多，其保证了至少一个数字的输入。

这两个限定符可以与任何字符或元字符连用。比如，`a+` 一个或更多a。`[abc]+` 一个或更多a、b、c其中一个字符。 `.*`0个或更多任意字符。

例子：

Match   aaaabcc 
Match   aabbbbc
Match   aacc  
Skip    a

> `a+b*c+`;或 `a{2,4}b{0,4}c{1,2}`;

# 第八课   可选元字符 ？

一个可以提供可选择性的限度符是`?`。
这个元字符是你可以这个符号之前的一个字符匹配或不匹配，它使它之前的一个字符可选匹配。例如，`ab?c`可以匹配`ac`或`abc`因为b是可选匹配。
如果你想匹配问号，则要使用`\?`来匹配。

例子:
Match   1 file found?
Match   2 files found?
Match   24 files found?

> `\d+ files? found\?`

# 第九课 空格 \s \S

在现实文本输入的场景中，很难遇不见空格。在正则表达式中最常见的包含空格的形式是空格(space)、制表符(\t)、换行符(\n)、返回符(\r)，以上所有字符都对应自己的空格形式。
另外，`\s`符匹配以上所有白空格内容。
`\S`匹配所用非白空格内容。

例子：
Match   1.   abc
Match   2.  abc
Match   3.           abc
Skip    4.abc

> `\d\.\s+\w+`

# 第十课 开始于结束 ^....$

假设我们想在日志文件中匹配单词"success"，我们当不想匹配到这么一行"Error: unsuccessful operation"。所以大多数情况下，我们的正则表达式要写的越准确越具体越好。

一个把匹配表达式写的更紧凑的方法是使用元字符`^`和`$`来限定句子的开始和终止。在上面的例子中，我们可以用`^success`来匹配仅仅以"success"开头的行，加上`$`你可以限定一句的开始和结尾。

注意`^..`与`[^..]`是完全不同的用法。

例子:
Match   Mission: successful
Skip    Last Mission: unsuccessful 
Skip    Next Mission: successful

> `^Mission: successful$`

# 第十一课 组匹配 ()

使用正则表达式我们不仅可以匹配文本，也可以抽取信息以便进一步处理。它可以通过定义字符组并使用元字符`(`与`)`。任何的在括号中的子模式将被当作组而被捕获。在实践中，组匹配常用于在各类信息中筛选像电话号码、电子邮件类的信息。

考虑这种情况，使用命令行列出储存在云盘上的所有图片文件，你可以用这样的匹配模式`^(IMG\d+\.png)$`捕获所用的图片文件名。如果你仅仅想捕获文件名可以用`^(IMG\d+)\.png$`这样可以捕获扩展名之前的内容。

例子：

Capture file_record_transcript.pdf 
Capture file_07241999.pdf
Skip    testfile_fake.pdf.tmp

> `^(file.+)\.pdf$`

# 第十二课 组匹配嵌套 ( .. (..) .. )

当在对复杂的数据操作的时候，要抽取多层次的数据是很常见的，这就会产生组匹配嵌套。一般的，组匹配嵌套捕捉的结果顺序就是它们定义时的顺序。

以上一课捕捉图片文件名的例子来说，如果每一个图片文件名中都存在一段数字标号，我们可以用这样的匹配模式`^(IMG(\d+))\.png$`。

嵌套的组被从左到右读取，顺序即是按左括号出现的顺序。

例子：

Capture Jan 1987    Jan 1987 1987  
Capture May 1969    May 1969 1969
Capture Aug 2011    Aug 2011 2011

> `(\w+\s(\d+))` 

# 第十三课 更多的组匹配内容 (.*)匹配全部

我们之前学过的限定符包括*、+、{}、？等可以作用于组匹配模式上，这是对字符串作用限定符的唯一方法，除此之外，限定符只能作用于单个字符上。

比如，如果我不确定一个电话号码是否包含区号，正确的匹配区号的模式为`(\d{3})?`。

例子：
Capture 1280x720    1280 720
Capture 1920x1600   1920 1600
Capture 1024x768    1024 768

> `(\d+)x(\d+)`

# 第十四课 或 |

之前我们说个，写正则表达式时最好要写的准确。
当我们有多个确定的可能选项时，使用`|`或符来连接多个可能性的模式。比如，匹配`(milk|break|juice)`表示匹配这三个可能性结果中的一个。

你也可以结合或符与其他元字符、限定符一起使用。比如，`([cb]ats*|[dh]ogs?)`表示匹配cats或bats或dogs或hogs。如果条件写的太多，会降低模式的可读性，可以考虑写成多个独立的模式。

例子：

Match   I love cats
Match   I love dogs
Skip    I love logs
Skip    I love cogs

> `I\slove\s(cats|dogs)` 或 `I love (cats|dogs)`

# 第十五课 其他特殊字符 \b

这节课介绍其他的元字符，和组匹配的结果。

我们之前学过使用`\d`捕获数字，使用`\s`捕获空格，使用`\w`捕获数字字母字符。正则表达式也提供了匹配这些字符集补集的方法，通过使用这些元字符的大写字母，可以表示原本小写字母表示集的补集。

另外，`\b`元字符匹配单词的界限，`\b`的长度为0，所以其不匹配任何字符，只是匹配单词边界。例如，`\bcat\b`可以匹配cat这个单词，但不会匹配包含cat的单词如catatonic，tomcat。`\bcat`会匹配以cat开头的单词catfish，`cat\b`会匹配cat结尾的单词tomcat。

很多系统允许你通过索引操作组匹配的结果，`\0`一般表示所有被匹配到的文本，`\1`第一组匹配结果，`\2`第二组匹配结果，等等。

# 练习一 匹配十进制数字

看题目为匹配十进制数字，你会认为很简单吧。

我们用`\d`匹配任意数字，通过它来匹配十进制数字应该很容易吧？对于简单的数字当然很容易，但如果是匹配用科学计数法写出来的数字，或金融表示的大数字，你需要应对正负号，有效位，指数表示，甚至不同的表示格式(如用，号分割千位，百万位)。

例子：
Match   3.14529
Match   -255.34
Match   128
Match   1.9e10
Match   123,340.00 
Skip    720p

> `^-?\d+(,\d+)*(\.\d+(e\d+)?)?$`

# 练习二  匹配电话号码

手机号码匹配也是一个需要技巧的任务。在不同的州需要加区号，国际长途会有国家前缀号，这些都会增加表达式复杂度。另外，不同人也有不同的书写号码的习惯(有些人会在号码中加入横线或空格分隔号码)。

例子：
Capture 415-555-1234    415
Capture 650-555-2345    650
Capture (416)555-3456   416
Capture 202 555 4567    202
Capture 4035555678      403
Capture 1 416 555 9292  416

> `1?[\s-]?\(?(\d{3})\)?[\s-]?\d{3}[\s-]?\d{4}`

# 练习三 匹配电邮

在HTML格式中，使用正则表达式获取数据很有用。特别的，电子邮件由于形式构成的复杂性，正则匹配起来会有难度，作者推荐使用内建语言或函数处理，而不是自己写表达式。 当然是用我们所学的内容，你也可以写出一个鲁棒性强的匹配表达式。

一个要注意的事情是，很多人用加地址来当作用一次的电邮地址。比如，`name+filter@gmail.com`的邮件会直接发给`name@gmail.com`但可以用附加的信息过滤此邮件。另外，有些域名有多个部分组成，也需要注意。


Capture tom@hogwarts.com    tom
Capture tom.riddle@hogwarts.com tom.riddle 
Capture tom.riddle+regexone@hogwarts.com    tom.riddle
Capture tom@hogwarts.eu.com tom
Capture potter@hogwarts.com potter
Capture harry@hogwarts.com  harry
Capture hermione+regexone@hogwarts.com  hermione
> `^([\w\.]*)`


# 元字符表

Metacharacter |  Description
---           | ---
\             | 
^             | Matches the position at the beginning of the input string.
$             | Matches the position at the end of the input string.
*             | Matches the preceding subexpression zero or more times.
+             | Matches the preceding subexpression one or more times.
?             | Matches the preceding subexpression zero or one time.
{n}           | Matches exactly n times, where n is a non-negative integer.
{n,}          | Matches at least n times, n is a non-negative integer.
{n,m}         | Matches at least n and at most m times, where m and n are non-negative integers and n <= m.
.             | Matches any single character except "n".
[xyz]         | A character set. Matches any one of the enclosed characters.
x|y           | Matches either x or y.
[^xyz]        | A negative character set. Matches any character not enclosed.
[a-z]         | A range of characters. Matches any character in the specified range.
[^a-z]        | A negative range characters. Matches any character not in the specified range.
b             | Matches a word boundary, that is, the position between a word and a space.
B             | Matches a nonword boundary. 'erB' matches the 'er' in "verb" but not the 'er' in "never".
d             | Matches a digit character.
D             | Matches a non-digit character.
f             | Matches a form-feed character.
n             | Matches a newline character.
r             | Matches a carriage return character.
s             | Matches any whitespace character including space, tab, form-feed, etc.
S             | Matches any non-whitespace character.
t             | Matches a tab character.
v             | Matches a vertical tab character.
w             | Matches any word character including underscore.
W             | Matches any non-word character.
un            | Matches n, where n is a Unicode character expressed as four hexadecimal digits. For example, u00A9 matches the copyright symbol
