---
categories:
- 运维
date: "2026-07-25T08:00:12+08:00"
draft: false
lastmod: "2026-07-25T15:28:12+08:00"
slug: codex-in-wsl
tags:
- codex
- wsl
- linux
title: wsl中配置codex
---

安装wsl并设置用户名和密码之后，先从开始菜单搜索启动wsl setting，在`网络`设置中将第一行的`nat`改为`Mirrired`。这样设置后，wsl 相当于host模式的docker，并且可以直接使用代理软件。先关闭wsl：`wsl --shutdown`，然后通过`wsl`启动，完成重启。
下面正式开始配置codex + python环境。

先安装一些必要的工具

``` bash
sudo apt update & apt install unzip nano git
```

## nodejs

第一步就是安装[Node.js --- 下载 Node.js®](https://nodejs.org/zh-cn/download)，选择linxu-fnm-npm-LTS的安装方式，给出以下安装指令。

但是安装脚本依赖

``` bash
# 下载并安装 fnm：
curl -o- https://fnm.vercel.app/install | bash

# 刷新终端
source /home/szc/.bashrc

# 下载并安装 Node.js：
fnm install 24
# 验证 Node.js 版本：
node -v # Should print "v24.xx.xx".
# 验证 npm 版本：
npm -v # Should print "11.16.0".
```

这样nodejs就安装完成了

## 下面先安装codex

``` bash
npm i -g @openai/codex
```

根据代理的情况，我这1min左右就安装完毕。也有可能安装5mins。

使用`codex login`启动登录，会在主体的浏览器打开一个登录网页，登录即可。

## python

下面的配置uv其实可以通过codex执行，这里给出参考过程
根据[Installation \| uv](https://docs.astral.sh/uv/getting-started/installation/#installation-methods)，linux 的安装指令为：

``` bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

需要更新path，执行`source $HOME/.local/bin/env`

安装python
`uv python install 3.12`

我常用3.12版本，这里固定版本
`uv python pin 3.12`

## R语言

这里我还配置了R 语言，专门用于绘图。根据官网[R: The R Project for Statistical Computing](https://www.r-project.org/)的提示，可以去镜像站下载。[清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/CRAN/)提供的ubuntu安装指令如下：

``` bash
# update indices
sudo apt update -qq
# install two helper packages we need
sudo apt install --no-install-recommends software-properties-common dirmngr
# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: E298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# add the repo from CRAN -- lsb_release adjusts to 'noble' or 'jammy' or ... as needed
# 注意！！！ 这一步需要按ENTER 同意安装
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
# install R itself
sudo apt install --no-install-recommends r-base
```

具体的配置参考 <a href="AI配置R 绘图" class="wikilink">AI配置R绘图</a>

## 在资源管理器中查看wsl文件

只需要在wsl终端中，`explorer.exe .`
