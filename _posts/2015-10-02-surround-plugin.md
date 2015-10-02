---
layout: post
title: Surround Plugin
description: 在这里分别介绍一下Vim和Sublime Text中在编写代码的时候能提高效率的插件，它的作用是能对(),[],"",'',XML标签等符号进行快速操作。
tag: surround,vim,sublime text
---

在这里分别介绍一下**Vim**和**Sublime Text** 中在编写代码的时候能提高效率的插件，它的作用是能对(),[],"",'',XML标签等符号进行快速操作。

##**Vim**
Vim 的 Surround 插件的相关信息可以Github的插件官方<a href="https://github.com/tpope/vim-surround" target="_blank">主页</a>中查看，上面提供了使用方法和下载方式，不过下载方式只说明了用 git clone 方式，我用的是<a href="https://github.com/VundleVim/Vundle.vim" target="_blank">Vundle</a>安装，更加方便。

利用官方的例子，像 Hello World! 这样的字符串进行增、删、改包围该字符串的符号

* ### **Add**
在**Visual**模式下选中字符串，按下shift+s，输入想要包围的符号，再回车即可，如图所示:<br><img src="{{ site.url }}/images/surround-add.gif" alt="surround-add">

* ### **Delete**
将光标移动到字符串所在行，在**Visual**模式按下ds，再按下想要删除的符号即可(助记:Delete Surround)。删除符号的规则是，优先删除光标所在处之后的第一个包围符号，如'',""若包围符号是(),{}等，只删除光标当前的。如图所示: <br><img src="{{ site.url }}/images/surround-delete.gif" alt="surround-delete">

* ### **Change**
操作方式和“删”类似，将光标移动到被符号包围的字符串中，按下cs，再按下需要修改的符号，最后按下替换的符号即可(助记:Change Surround)，如图所示：<br><img src="{{ site.url }}/images/surround-change.gif" alt="surround-change">

值得一提的是，从上面的动图可以观察到，在增改符号的时候，有些符号例如(),[],{}等闭合的符号，输入左边和右边符号的效果是不一样的，输入左边的符号会增加一个空格。

另外，在选取要操作的字符串时，插件还支持用Vim里的光标motion选择，例如:

* ysiw'的效果是选择当前光标的word，然后用'符号包围;
* ysa')表示选择当前''(包含'')的字符串，然后用()包围;
* 如果是对整行字符串操作，则用yss选择整行，再输入要包围的符号即可
 <br><img src="{{ site.url }}/images/surround-motion.gif" alt="surround-motion">

##**Sublime Text**
Sublime Text 中的 Surround 插件的功能和Vim的一样，只是用法稍微有点区别，插件<a href="https://github.com/jcartledge/sublime-surround" target="_blank">主页</a>也有很详细的安装和使用说明。

同样以 Hello World! 举例增删改的符号操作，就不详细说明了，动图可以看得很明白: <br>

<center><img src="{{ site.url }}/images/surround-sublime.gif" alt="surround-sublime"></center>

想说明一下的是，对于给字符串增加'',"",(),{}这些符号，Sublime自身就具有这样的功能，具体操作是选中该字符串，输入上述符号的左半部分即可，输入右半部分并没有上文提到的加空格的效果。如果是处理HTML的标签的话，推荐使用**Emmet**插件，效率更高。此外，Sublime Text 的 Surround 插件也提供 Vintage 版的，需要配合Surround插件来使用，安装也很简单，使用Package Control即可，使用方法除了增加符号时使用的是小写的s之外，其他操作完全一样，这也是在 Sublime Text 中Vintage模式的使用者的一个好工具。