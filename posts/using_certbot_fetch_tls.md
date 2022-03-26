---
title: "使用 Certbot 为网站添加不会“过期”的 TLS 证书"
date: 2022-03-26T01:40:12+08:00
weight: 2
tags: ["Linux"]
author: "Guanyu"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: ""
canonicalURL: "https://canonical.url/to/page"
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowTags: true
ShowWordCount: false
ShowReadingTime: false
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "https://letsencrypt.org/images/letsencrypt-logo-horizontal.svg" # image path/url
    alt: "let's encrypt logo" # alt text
    caption: "Let’s Encrypt Logo (CA)" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

绝大多数云服务提供商本身就有 TLS 证书颁发服务，但是免费的 TLS 证书有效期限仅有一年，且有数量限制，到期后需要重新申请证书、下载再配置到服务器上，每年重新弄一次。

[Certbot](https://certbot.eff.org/) 可以帮助我们自动从 [Let’s encrypt](https://letsencrypt.org/) 申请 TLS 证书，到期后自动重新申请新证书，并且这一过程不需要人工去操作，十分的人性化（真的不是我懒，你听我给你狡辩 😛）



### 什么是 Let‘s encrypt

[Let’s encrypt](https://letsencrypt.org/) 是一个非盈利性的 TLS 证书颁发机构（CA），只要你能“证明”这个域名属于你，就可以从 [Let’s encrypt](https://letsencrypt.org/) 上申请证书 。

所谓“证明”就是在你的 Web 主机上运行 ACME 协议的软件帮你去获取证书的过程，后文要安装的 [Certbot](https://certbot.eff.org/) 就是一种 ACME 软件。

虽然说 [Let’s encrypt](https://letsencrypt.org/) 提供的域名有效期仅有三个月，但是 [Certbot](https://certbot.eff.org/) 会自动帮我们完成新证书的申请，所以就没我们什么事了 😆。



### 安装 Certbot

安装 [Certbot](https://certbot.eff.org/) 有三个前置条件，在官网上也有很详细的说明。

- 熟悉命令行操作
- 你已经有一个在 80 端口可访问的 HTTP 服务
- 你能访问服务所在的服务器，并且拥有 sudo 权限


如果你的环境和我不一样可以按照官方的引导进行操作，在官网首页选择你自己的环境配置。这里我们选择的是：

```shell
My HTTP website is running Nginx on CentOS 8
```


我们需要先安装一个叫做 Snapd 的包管理软件，通过 Snapd 再去安装 Certbot（Certbot 是一个 snap 应用）。

但是在安装 Snapd 之前，我们需要先安装 EPEL (Extra Packages for Enterprise Linux) 仓库（禁止套娃），这是 Fedora 社区为 RHEL 系列衍生 Linux 系统提供的高质量软件仓库，相当于一个第三方的软件源，我们要安装的 Snapd 就在里面。

```shell
sudo dnf install epel-release
# 或者
sudo yum -y install epel-release
```

现在我们就可以安装 Snapd 了。

```shell
sudo yum install snapd
```


> 🚨 注意：这里刚安装完 Snapd 服务是没有运行的，直接使用 snap 命令会报错，需要先使用 `systemctl start snapd.service` 启动一下 Snapd 服务。


执行下面的命令确保 snapd 是最新的版本。

```shell
sudo snap install core; sudo snap refresh core
# 没有更新会提示以下内容，就可以进入下一步了
snap "core" has no updates available
```

开始安装 Certbot

```shell
sudo snap install --classic certbot
```

> 🔔 走到这里你可能会遇到一个错误（没遇到直接跳过）
> ```shell
> error: cannot install "certbot": classic confinement requires snaps under /snap or symlink from /snap to /var/lib/snapd/snap
> ```
> 处理办法也很简单，按照它的要求创建一个软链接即可
> ```shell
> sudo ln -s /var/lib/snapd/snap /snap
> ```


再为 Certbot 创建一个软链接保证 Certbot 命令能正常执行

```shell
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```


最后直接运行 Certbot，它会以命令行的形式让你输入一些必要的信息去做第一次的初始化

```shell
sudo certbot --nginx

# 提示你输入邮箱
Enter email address (used for urgent renewal and security notices)

# 第二步提示你是否同意 ACME 协议内容（不能拒绝）
You must agree in order to register with the ACME server. Do you agree?

# 第三步向你确认，是否能给你发送 Let's encrypt 相关的动态、新闻邮件（可以拒绝）

# 第四步选择你要生成 TLS 证书的域名
Which names would you like to activate HTTPS for?
```

没有意外的话你会看到 `Successfully deployed` 字样。这时再去检查 `nginx.conf` 会发现，Certbot 已经帮你把 TLS 证书相关的配置都添加上了，甚至贴心的帮你把 80 端口到 443 端口的重定向也配置好了，感动。

> PS: 「就是格式有点丑，缩进啥的没对上，强迫症表示无法接受。」
> 
> 😒 作者：「键盘给你，你来写。」




可以通过以下方式检查 Certbot 生成的自动化任务执行情况

- `/etc/crontab/` 
- `/etc/cron.*/*` 
- `systemctl list-timers` 



至此，只要你的服务器不宕机，以后就再也不用关心证书的问题啦！
