---
layout: post
title: CentOS下MSOffice转PDF
modified: 2015-5-2
categories: tech
tags:
 - CentOS
 - Word处理
comments: true
description: 在网站上显示文档的时候，pdf格式往往是比较便捷的处理格式。而平时在编辑时我们使用MSOffice更为便捷。在这种情况下，我们需要一种自动的将Office文档转换为pdf的方式。本文就是介绍了在CentOS6系统下Office转pdf工具的安装使用方法。
---
*更新：在CentOS上安装OpenOffice的时候其实默认安装的是Libre Office，所以之后如果要用yum装相关的扩展，需要指定libreoffice。比如安装字体扩展xsltfilter要写成“libreoffice-xsltfilter”而不是"openoffice.org-xsltfilter"。我已经对下文相关地方做出了修改。*

将Office文件转成其他格式并没有想象的那么方便，比较好的不需要使用商业软件的方法需要在服务器上安装OpenOffice或者LibreOffice，这里记录一下所需软件的安装过程：

## 一、安装环境
我使用CentOS6.6(64bit)服务器版（运行在Virtual Box虚拟机环境下）作为实验环境，正式使用时也是该版本。

## 二、安装OpenOffice
服务器通常情况下都是没有图形界面的。网上查资料的时候发现有些安装方法截然不同。我采用的是[参考文章1][1]的方法，具体步骤如下：

### Step 1.从仓库安装OpenOffice的headless版本

{% highlight Bash shell scripts %}
sudo yum install openoffice.org-headless openoffice.org-writer openoffice.org-draw
{% endhighlight %}

其中后面两个包是必须的，否则会得到`This url is not supported`这样的报错；

### Step 2.创建一个shell脚本使其能够作为服务运行：

{% highlight Bash shell scripts %}
sudo vim /etc/init.d/openoffice.sh
{% endhighlight %}

需要写入的脚本代码如下：
{% highlight Bash shell scripts %}
#!/bin/bash
# openoffice.org headless server script
#
# chkconfig: 2345 80 30
# description: headless openoffice server script
# processname: openoffice
#
# Author: Vic Vijayakumar
# Modified by Federico Ch. Tomasczik
#
OOo_HOME=/usr/bin
SOFFICE_PATH=$OOo_HOME/soffice
PIDFILE=/var/run/openoffice-server.pid

set -e

case "$1" in
start)
if [ -f $PIDFILE ]; then
echo "OpenOffice headless server has already started."
sleep 5
exit
fi
echo "Starting OpenOffice headless server"
$SOFFICE_PATH --headless --nologo --nofirststartwizard --accept="socket,host=127.0.0.1,port=8100;urp" & > /dev/null 2>&1
touch $PIDFILE
;;
stop)
if [ -f $PIDFILE ]; then
echo "Stopping OpenOffice headless server."
killall -9 soffice && killall -9 soffice.bin
rm -f $PIDFILE
exit
fi
echo "Openoffice headless server is not running."
exit
;;
*)
echo "Usage: $0 {start|stop}"
exit 1
esac
exit 0
{% endhighlight %}

保存并退出；

### Step 3.使该脚本能被运行

{% highlight Bash shell scripts %}
sudo chmod 0755 /etc/init.d/openoffice.sh
{% endhighlight %}

### Step 4.使该服务能在开机时自动运行

{% highlight Bash shell scripts %}
sudo chkconfig - -add openoffice.sh
{% endhighlight %}

有关chkconfig的用法可以参考[这篇文章][2]。
在Ubuntu下命令为：`sudo update-rc.d openoffice.sh defaults`

### Step 5.运行该脚本开启服务

{% highlight Bash shell scripts %}
sudo /etc/init.d/openoffice.sh start
{% endhighlight %}

### Step 6.检查OpenOffice是否在运行

{% highlight Bash shell scripts %}
netstat -nap | grep office
{% endhighlight %}

可以获得类似如下信息则表明运行成功：

{% highlight Bash shell scripts %}
tcp        0      0 127.0.0.1:8100              0.0.0.0:*                   LISTEN      1092/soffice.bin
{% endhighlight %}

## 三、安装UNOCONV
unoconv是一个不错的文档转换工具。具体介绍可以参考其[官网][3]。安装步骤如下：

### Step 1.安装pyuno
由于UNOCONV使用python，因此需要安装其依赖——OpenOffice中的pyuno:

{% highlight Bash shell scripts %}
sudo yum install openoffice.org-pyuno
{% endhighlight %}

另外再下几个包，例如支持中文的`libreoffice-langpack-zh_CN.x86_64`以及`libreoffice-xsltfilter.x86_64`。之后如果有相关依赖可以使用`yum search libreoffice`进行搜索。

### Step 2.下载安装UNOCONV
RedHat系列rpm包下载页面[在此][4]，我下载的是0.5版本：

{% highlight Bash shell scripts %}
wget http://pkgs.repoforge.org/unoconv/unoconv-0.5-1.el6.rf.noarch.rpm
sudo rpm -ivh rpm -ivh unoconv-0.5-1.el6.rf.noarch.rpm
{% endhighlight %}

若按照前面的步骤下来，应该不会有报错，执行`unoconv —version`显示如下信息：

{% highlight Bash shell scripts %}
unoconv 0.5
Written by Dag Wieers <dag@wieers.com>
Homepage at http://dag.wieers.com/home-made/unoconv/

platform posix/linux2
python 2.6.6 (r266:84292, Jan 22 2014, 09:42:36)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)]
LibreOffice 4.0

build revision $Rev$
{% endhighlight %}

## 四、安装中文支持
到此，我们可以测试一下.doc转pdf是否成功：

{% highlight Bash shell scripts %}
unoconv -f pdf test.doc
{% endhighlight %}

如果测试的文档中包含中文，那么在生成的pdf中中文是不会被正确显示的，原因就在于没有安装MS中文支持。可以按照如下步骤安装支持，其中用到的软件都是开源或者有使用许可的。其中关于是否可以使用msttcorefonts可以参见[这个回答][6]。

### Step 1.安装CentOS中的中文支持
注意在CentOS 6中安装和之前的方法不同，具体可以参考[How to Install Asian Fonts on Red Hat & CentOS Linux Distributions][7]。具体命令为：

{% highlight Bash shell scripts %}
yum groupinstall "Chinese Support"
{% endhighlight %}

### Step 2.安装微软中文字体支持：[cabextract][8]和[msttcorefonts][9]

#### 2.1 安装cabextract：
{% highlight Bash shell scripts %}
yum install cabextract
{% endhighlight %}

如果出现找不到这个软件的问题，可以用rpm安装一个`epel-release`包：`sudo rpm -Uvh http://mirrors.sohu.com/fedora-epel/5/i386/epel-release-5-6.noarch.rpm`。（注意这里是5.4版本，可以下载最新版本，如果今后在安装其他软件过程中epel包较旧，可以卸载旧版本安装最近版本。）然后再执行`yum install cabextract`。

#### 2.2 安装msttcorefonts：

{% highlight Bash shell scripts %}
wget http://corefonts.sourceforge.net/msttcorefonts-2.5-1.spec
{% endhighlight %}

该步骤下载了一个.spec文件，但该文件中的mirrors过老（修改时间为2012版时），有些需要的文档无法下载，可以通过修改其中的镜像的路径：定位到62行，将`http://${m}.dl.sourceforge.net/project/corefonts/the%20fonts/final/`改为`http://sourceforge.net/project/corefonts/the%20fonts/final/`。
其次执行：

{% highlight Bash shell scripts %}
rpmbuild -bb msttcorefonts-2.5-1.spec
{% endhighlight %}

在此过程中文件`wd97vwr32.exe`不能从sourceforge镜像中下载到，可以在别的地方下载好后放入`$HOME/rpmbuild/BUILD/msttcorefonts/downloads/`。
安装rpm包：

{% highlight Bash shell scripts %}
rpm -ivh $HOME/rpmbuild/RPMS/noarch/msttcorefonts-2.5-1.noarch.rpm
{% endhighlight %}

### Step 3. 更新字体缓存

{% highlight Bash shell scripts %}
fc-cache -f -v
{% endhighlight %}

## 四、准备就绪
到目前为止所需的准备工作均已完成。我们已经可以在命令行环境中对MSOffice文档进行转换。

[1]: http://www.openvpms.org/documentation/install-openoffice-headless-service-ubuntu
[2]: http://www.cnblogs.com/panjun-donet/archive/2010/08/10/1796873.html
[3]: http://dag.wiee.rs/home-made/unoconv/
[4]: http://pkgs.repoforge.org/unoconv/
[5]: http://oommgg.net/2013/04/使用-unoconv-將-doc-xls-ppt-轉換成-pdf/
[6]: http://askubuntu.com/questions/134549/is-it-legal-to-install-msttcorefonts-package-is-wine-legal
[7]: http://help.accusoft.com/PCC/v8.3/HTML/PCCIS_How%20to%20Install%20Asian%20Fonts%20on%20Red%20Hat%20and%20CentOS%20Linux%20Distributions.html
[8]: http://www.cabextract.org.uk
[9]: http://corefonts.sourceforge.net




