---
layout: post
comments: false
categories: lua
---

前天看到一篇新闻《[日本早稻田大学发布自动描线与自动上色技术](http://news.missevan.com/news/article?newsid=40092)》，关键是自动上色技术是开源的。所以这里忍不住先尝尝鲜。

我也是第一次接触lua，下面是本人的配置过程(环境为Ubuntu14.04)，可能有些遗漏或错误请见谅。

首先是github上的[源码](https://github.com/satoshiiizuka/siggraph2016_colorization)和[文档](http://hi.cs.waseda.ac.jp/~iizuka/projects/colorization/en/)。


1\. 下载源码

```
git clone https://github.com/satoshiiizuka/siggraph2016_colorization.git
```

2\. 执行源码中的download_model下载模块

```
./download_model.sh
```

3\. 安装luarocks（参考[这里](http://torch.ch/docs/getting-started.html)）

```
wget http://luarocks.org/releases/luarocks-2.3.0.tar.gz

tar -zxvf luarocks-2.3.0.tar.gz

cd luarocks-2.3.0

./configure --prefix=/usr/local/luarocks
```

在这里碰到第一个问题

<font color='red'>Checking Lua includes... lua.h not found (looked in /usr/include, /usr/include/lua/5.1, /usr/include/lua5.1)</font>

原因是没有lua，所以先安装lua

```
wget http://www.lua.org/ftp/lua-5.1.2.tar.gz

tar xzvf lua-5.1.2.tar.gz

cd lua-5.1.2

make linux

make install
```

在安装lua过程中可能碰到的问题有：

a\. 下载时碰到<font color='red'>Resolving www.lua.org (www.lua.org)... failed: Name or service not known.</font>

解析不了域名，改一下`/etc/resolv.conf`，改成谷歌的DNS服务器

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

b\. 执行`make linux`时碰到<font color='red'>luaconf.h:275:31: fatal error: readline/readline.h: No such file or directory</font>

需要先安装readline支持

```
apt-get install libreadline5

apt-get install libreadline-dev
```

c\. 同样执行`make linux`时遇到的错误<font color='red'>/usr/bin/ld: cannot find -lncurses</font>

先安装curses库

```
apt-get install libncurses5-dev
```

4\. 安装完lua再回到luarocks目录下执行一次`./configure --prefix=/usr/local/luarocks`，这次应该可以了

那么下一步就是make

```
make build

make install
```

5\. 装完luarocks，最后再安装3个支持库。

```
luarocks install image

luarocks install nngraph

luarocks install nn
```

一个都没装成？好吧，看看错误信息。<font color='red'>-bash: /usr/bin/luarocks: No such file or directory</font>

没有指令，安装到别处了？那就链接过来（可以通过whereis或者locate指令找到路径）

```
ln -s /usr/local/luarocks/bin/luarocks /usr/bin/luarocks
```

再跑一次。还是有问题……<font color='red'>lnn.c:4:23: fatal error: nanomsg/nn.h: No such file or directory</font>

嗯，还要先安装torch

```
git clone https://github.com/torch/distro.git ~/torch --recursive

cd ~/torch

./install.sh

source ~/.bashrc

source ~/.zshrc

source ~/.profile
```

再次执行上面那3个`luarocks install`，总算可以了。

6\. 需要的支持库都齐了！回到一开始的项目目录下，准备见证传闻的黑科技

```
th colorize.lua ansel_colorado_1941.png out.png
```

这里可能出现问题

<font color='red'>/root/torch/install/bin/luajit: /root/torch/install/share/lua/5.1/torch/File.lua:370: table index is nil
stack traceback:

...

colorize.lua:23: in main chunk</font>

这个估计是编码问题，打开colorize.lua文件，将第23行修改为：

```
local d = torch.load( 'colornet.t7', 'b64')
```

保存后再执行。坐等结果就行

* * *

作者在最后也提示了如果图片过大可能爆内存（反正我1G内存直接是爆了，错误信息里还温馨提示了BUY NEW RAM，宝宝心好累）。
在朋友电脑上跑过确实是得到了极赞的效果。这里不放效果图，留个想象空间（嘿嘿）。