---
layout: post
title: Sublime Text Tips & Tricks
description: Sublime Text 这款编辑器的优秀特性大家有目共睹，不多做介绍，Sublime Text的配置是百分百可以自定义的，在这里分享个人在使用过程中经常使用的一些能提高工作效率的小技巧，熟练运用这些小技巧可以节省在写代码过程中的大量时间。在此假定读者已经具备使用Package Control安装插件的能力。
tag: sublime
---

[Sublime Text](http://www.sublimetext.com/) 这款编辑器的优秀特性大家有目共睹，不多做介绍，Sublime Text的配置是百分百可以自定义的，在这里分享个人在使用过程中经常使用的一些能提高工作效率的小技巧，熟练运用这些小技巧可以在写代码过程中节省大量的时间。在此假定读者已经具备使用Package Control安装插件的能力。

##配置文件(Settings)

* ####系统配置
点击界面菜单选项 <em>Preferences</em>-><em>Settings-Default</em>,文件内容是有关主题、字体、缩进等的设置
* ####快捷键配置
点击界面菜单选项 <em>Preferences</em>-><em>Key Bindings-Default</em>,文件内容是有关如打开文件(`Ctrl + o`),新建文件(`Ctrl + n`)等快捷操作

以上提到的两个配置文件是系统默认的，而且都有各自对应的`User`文件，主要是用户添加符合自己习惯的配置项，如要修改配置文件，建议在`User`文件中添加内容进行覆盖，这样的好处是如果要对编辑器进行升级，`User`文件的内容不会被覆盖。上面提到的一些操作都很简单，这并不是重点，接下来还会提到里面的一些配置选项
##工程(Project)

菜单选项<em>Project</em>里面是有关工程的相关配置，先将工程保存为<em>.sublime-project</em>的文件，值得一提的是，系统配置文件里的所有选项都可以添加到工程文件中

* <em>"folder_exclude_patterns": [""]</em>和<em>"file_exclude_patterns": [""]</em>这两个选项比较常用，其中一个应用场景是在MVC框架中，前端和后台的用户可以选择性地显示左侧 Sidebar`(Ctrl+k,Ctrl+b)` 中的文件夹和文件，支持通配符，一个整洁的文件列表可以有效地选择所需的文件。
* Sublime 里可以在<em>Tools</em>-><em>Build System</em>配置 Build System,也可以在工程文件里配置对应的 Build System，这样可以在Java，PHP等不同语言项目环境中直接调试程序。
* `Ctrl + Alt + p` 快捷键可以快速切换工程。
<center><img src="{{ site.url }}/images/project-exclude.gif" alt="project-exclude"></center>
<center><img src="{{ site.url }}/images/project.gif" alt="project"></center>

##插件(Plugin)

* ####AdvancedNewFile
插件功能是快速建立所需文件夹或者文件。我们平时建立新文件时一般是敲入`Ctrl+n`输入内容然后保存文件，再在对话框中选择要保存的文件夹，或者在左侧的Sidebar用鼠标选中目标文件夹右键点击`New File`再输入内容保存，这里有一个问题是，在新建文件时，如果想在目标文件夹下同时建立级联目录，并不能一步到位，比如想要在 `views`目录新增`front/index.php`这样的目录结构，右键`views`目录->`New Folder`，键入`front/index.php`得到的目录结构是`views/front/index.php/`，index.php文件并没有建立。这个插件可以方便建立级联文件，在该插件的 Key-Bindings 文件中可以看到相关快捷键是<em>Ctrl+Alt+n</em>,如果是在打开的文件中输入快捷键，界面下方的输入框会把路径默认设置成当前文件路径。可以使用和终端类似的`".."`符号进入上级目录，非常方便。

* ####Nettuts+ Fetch
插件功能是给特定链接建立别名，方便管理和下载自己定义的链接内容，下载内容可以是文本文件和压缩包。安装好插件后键入<em>Ctrl+Shift+p</em>打开Pallet，输入`fetch`，会出现File,Manage,Package这三个选项,Manage选项是对File和Package进行配置，对别名和链接配置完毕后，在Pallet中选择想要下载的链接别名即可。个人经常会把一些常用的配置文件、公共库文件和Github上面的一些项目进行添加管理。值得注意的是，在下载Package后，默认会把压缩包进行解压，省下不少功夫。<center><img src="{{ site.url }}/images/fetch.gif" alt="fetch"></center>

* ####HttpRequester
插件功能是快速在新文件中显示http请求报文，插件默认启用的快捷键是<em>Ctrl+Alt+r</em>，用法是选中请求地址，键入快捷键，请求完毕后会在新标签中显示http请求的报文信息，这样就可以省下在浏览器中进入控制台查看报文的操作，这样的功能在调试和调用接口的时候会用得比较多。<center><img src="{{ site.url }}/images/httprequester.gif" alt="httprequester"></center>

* ####EasyMotion
插件功能是快速移动光标至屏幕中所希望到达的位置，这个插件是从Vim移植过来的，功能并没有Vim下的强大，但是只需要快速移动光标至单字符的功能就已经足够。安装好插件后，在<em>Preferences</em>-><em>Package Settings</em>里找到EasyMotion选项可看到默认的启用快捷键是<em>Ctrl+;</em>，键入快捷键后再键入想要查找的字符，屏幕上会根据`0~9a-zA-Z`列出之前输入的字符，再敲入显示的字符即可将光标移动至该处，极大地解放了生产力。个人习惯将快捷键修改为<em>f+f</em>，在只使用一个f键启用快捷键带来便利的同时，也会带来一些小的不便，例如在输入类似"Offset"这样包含有两个f字母的单词或者代码语句时，得注意输入两个f字母的间隔时间，不过这样的事件很少会发生，<em>f+f</em>这样的方式利大于弊。如果屏幕中存在的搜索的字符超过0-9a-zA-Z的数量，其余的字符并不会显示，按下回车键后会显示下一组字符来定位。需要注意的是，在查找字符时，键入f并不能跳到该f所在的位置。<center><img src="{{ site.url }}/images/easymotion.gif" alt="easymotion"></center>

##多重光标(Multiple Cursor)
多重光标处理文本的效率相信大家都能体会到，在这里会延伸一两种使用多重光标的方法。PS:实在不知道怎么翻译Multiple Cursor。

* 最简单直接的一种使用方式是按住`Ctrl`键，再用鼠标点击想要编辑的地方，适用于编辑没有规律和少量的文本。

* ####`Ctrl+Alt+上/下方向键`
键入该快捷键后可以在输入方向的位置新增选择一个光标，需要注意的是，有些电脑键入上述快捷键后，经常会与系统其他程序的快捷键冲突，例如屏幕翻转，音量升降等，禁用或者更改相应程序快捷键即可，个人习惯将快捷键改成<em>Alt+Up/Down</em>。

* ####`Ctrl+d`
键入<em>Ctrl+d</em>后会选择光标所在内容，再输入一次可选择下一个相同的文本内容，直接键入<em>Alt + F3</em>可全选全文中光标所在内容。值得一提的是，在使用<em>Ctrl+d</em>时，可以使用<em>Ctrl+k</em>跳过选中的内容。<center><img src="{{ site.url }}/images/ctrl+d.gif" alt="ctrl+d"></center>

* ####正则表达式
在需要编辑一些有规律的文本时，考虑使用正则表达式是一个不错的选择。键入<em>Ctrl+f</em>后下方弹出搜索框，点击最左边的按钮或者键入<em>Alt+r</em>启用正则表达式搜索，剩下的工作就是看个人能力写出正则表达式，输入完表达式后，键入<em>Alt+Enter</em>即可选中匹配的内容。<center><img src="{{ site.url }}/images/re.gif" alt="re"></center>

* ####Vintage Mode + Multiple Cursor
使用 Vim 里的一些特性配合多重光标使用起来是非常高效的。Sublime Text 是以Package的方式支持 Vim 里的一些常用操作，默认支持和禁用 Vintage，在<em>Prreferences</em>-><em>Browse Packages</em>中可以找到Vintage这个Package，启用的方式是将如下代码添加至`Settings-User`文件中并保存，作用是如`ignored_package`表示的一样，把Vintage这个Package从忽略列表中去掉。<pre><code class="highlighter">
    "ignored_packages": [""]
</code></pre>
以下演示使用Vintage Mode两个常用的操作，一是选词，将光标置于词中的任何一个位置，键入<em>ciw</em>(Change Inner Word);二是根据位置删除文本，在光标同一行上，键入<em>dt"c"</em>(Delete Till Character,Character 代表任一字符)，就会将光标所在处至"c"之间的内容删除。Vim 中像这样的删除和改变文本的技巧还有很多，并不在本文的讨论范围。<center><img src="{{ site.url }}/images/vintage.gif" alt="vintage"></center>

## 总结

以上是个人对 Sublime Text 一些使用技巧的总结，目前使用 Sublime + Vintage 是本人编辑文本最高效的方式，由于长期使用Vim，已经离不开hjkl的光标移动方式，在编写代码时，由于得频繁地使用ESC键和方向键，分别移动左右手至该键是一件非常消耗体力和时间的事情，根据个人习惯，我把ESC键映射成了<em>;+;</em>，也看到有其他程序员将其映射成<em>Caps Lock</em>或者<em>j+j</em>键的，符合自己的习惯就好，所以在上面的动图中经常会出现敲入";"后马上消失的现象，以下是个人在 Subime Text 中常用的 Key-Bindings，会持续在[这里](https://github.com/lattespirit/default.sublime-keymap)更新。

<pre><code class="highliter">
  [
    { "keys": ["alt+up"], "command": "select_lines", "args": {"forward": false} },
    { "keys": ["alt+down"], "command": "select_lines", "args": {"forward": true} },
    { "keys": ["alt+d"], "command": "alignment" },
    { "keys": ["n","n"], "command": "advanced_new_file_new"},
    { "keys": [";",";"], "command": "exit_insert_mode",
            "context":
            [
                { "key": "setting.command_mode", "operand": false },
                { "key": "setting.is_widget", "operand": false }
            ]
    },
    { 
      "keys": [";"], "command": "exit_visual_mode", "args": {"toggle": true},
      "context":
      [
        {"key": "setting.command_mode"},
        {"key": "selection_empty", "operator": "equal", "operand": false, "match_all": false}
      ]
    },
    { 
      "keys": ["f","f", "<character>"], 
      "command": "easy_motion",
      "args": {"select_text": false} 
    }
  ]
</code></pre>

希望以上内容会有所帮助，如有更加有效的操作，欢迎交流指导。