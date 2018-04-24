---
title: SublimeText+Latex-ConfigurationOnMac
date: 2016-10-10 11:25:58
tags:
---

### 前言(Preface)
某日，突发奇想欲用Latex模版制作简历，由于开始研究配置，经过一番折腾终于弄好，然其过程不畅快使人折磨。故写此博文，以便诸君。
本文适用场景：基于 **BasicTex(2016)** 在 **SublimeText3** 中用 **LatexTool** 编译，于 **skim** 中显示的latexPDF。
写于2016年10月

##### 1.安装BasicTex
不需要安装完整版程序只要Basic功能就行。[BasicTex](http://mirror.ctan.org/systems/mac/mactex/mactex-basic.pkg)
如果安装没问题请看下一单元，如果网页打不开，或者下载包pkg打不开请用homebrew安装

- homebrew安装
1.在终端输入
`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" < /dev/null 2> /dev/null ; brew install caskroom/cask/brew-cask 2> /dev/null`
2.输入
`brew cask install basictex`
如果homebrew安装出现写入权限的问题(permisson denied),参考[此处](http://stackoverflow.com/questions/16432071/how-to-fix-homebrew-permissions)第一个答案。

- 安装latexmk
终端输入：
`sudo tlmgr update --self`
`sudo tlmgr install latexmk`

PS：如果终端不识别tlmgr命令说明要给系统增加环境变量。方法如下：
默认情况下tlmgr在目录`/usr/local/texlive/2016basic/bin/universal-darwin`中，如果不是默认情况请在`/`目录下找具体安装目录
在终端下输入：
`cd`
`vim .bash_profile`
进入文件，在文件底部添加：
`# Add TeXlive`
`export PATH=/usr/local/texlive/2016basic/bin/universal-darwin:$PATH`
输入`:wq`回到终端之后输入：
`source .bash_profile`
回车
然后再运行安装latexmk的两条命令就可以了。

##### 2.配置SublimeText
在ST里打开LaTeXTools.sublime-usersettings。在`builder-settings`中添加两项：
`"program" : "xelatex",`
`"command" : ["latexmk", "-cd", "-e", "$pdflatex = 'xelatex -interaction=nonstopmode -synctex=1 %S %O'", "-f", "-pdf"],`
LaTeXTools默认调用Skim来打开生成的PDF文件，如果你更喜欢使用OS X自带的“预览”，现在可以直接在用户设置中添加：
`"viewer": "preview",`
编译之后就是用preview显示了。

 
