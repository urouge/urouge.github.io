---
layout: post
title: Windows下修改 Vagrant Box 加载目录
tag: vagrant
---
加载目录默认在 ~/.vagrant.d/

<pre><code class="highlighter">setx VAGRANT_HOME "/your/path" /M</code></pre>

Also, you can check the registry (WinKey+R and type: regedit) at: HKEY_CURRENT_USER/Environment you should see a VAGRANT_HOME key if you added using SETX command, or you can add it manually here by doing:
- Right Click on the right panel, select New->String Value and name it VAGRANT_HOME.
- Add your path to the value you added on the previous step.


[Vagrant官网相关环境变量](http://docs.vagrantup.com/v2/other/environmental-variables.html)