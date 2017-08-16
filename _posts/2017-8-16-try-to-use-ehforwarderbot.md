---
layout: post
title: Try to Use EH Forwarder Bot (with CentOS)
comments: true
tags: tech
description: Make life cool~
categories: blog
---
EH Forwarder Bot can receive and send WeChat information on Telegram. See [EH Forwarder Bot – 在 Telegram 收发「微信」消息][1]. It's full doc is [EH Forwarder Bot][2]. For some reasons, I need a VPS to build the bot for me. This article is a note for the whole progress I build it.

### 1. Installation
My VPS OS is CentOS 6, which unfortunately not recommended by the Doc. Here are some issues and solutions.

1. Install libopus: add a repo source, see [libopus-1.0.1-1.el6.x86_64.rpm][3];
2. Install libmagic: I didn't find a good way to install libmagic alone on CentOS, but `file-devel` package has something necessary for programs using libmagic. See [Where can I download "libmagic" for fedora][4]
3. Install ffmpeg: it spent me a little bit more time but also easy to solve. Just follow what [How to Install FFmpeg on CentOS][5] says. If there are some issues about dependencies, just update the system on step 1 in the article.
4. [Tip] Don't forget to install `python34-setuptools` and `python34-devel`, and go to `/usr/lib/python3.4/site-packages/`, excute `python3 easy_install.py pip` to install pip3.

After excute `pip3 install -r requirements.txt`, all the installments have been done.

### 2. Getting Started
In order to use EFB, all you should do is editing `config.py`, which described in [Getting Started][6] section. However, before that, you should have setup your **master channel** and **slave channel**, which need more configuration. Just like below:

1. Setup Telegram Master.
2. Setup WeChat slave.


[1]: http://www.appinn.com/eh-forwarder-bot/
[2]: https://ehforwarderbot.readthedocs.io/en/latest/
[3]: https://centos.pkgs.org/6/linuxtech/libopus-1.0.1-1.el6.x86_64.rpm.html
[4]: https://forums.fedoraforum.org/archive/index.php/t-169345.html
[5]: https://www.vultr.com/docs/how-to-install-ffmpeg-on-centos
[6]: https://ehforwarderbot.readthedocs.io/en/latest/getting-started/
