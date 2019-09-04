---
tags: [GitHub]
title: 自制 CaptivePortal 检测服务器
created: '2019-09-04T08:22:28.082Z'
modified: '2019-09-04T08:22:30.924Z'
---

# 自制 CaptivePortal 检测服务器

2018\-10\-25 0条评论 339次阅读 0人点赞

当你使用国外的手机时，会发现你的 WIFI 图标上一直都有个叹号提示你”此网络无法连接到互联网”，其实我们可以用自建 http 204 服务器迅速解决这个问题。

---

## 1、制作准备

### 准备工具

1、Android Debug Bridge 已开启的手机
2、Android Debug Bridge 电脑端 \[[Windows 下载链接](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)\]

### 前提环境

1、一台 VPS 服务器，已搭建 Nginx。
2、公网环境可用的 web 服务器
3、支持 Curl 命令进行测试

---

## 2、制作步骤

### 2.1、搭建 Nginx 页面

当然你首先需要知道 Nginx 的基本站点配置结构，这部分就不叙述了，你可以参考文末链接。
我们只需要在其中增加一条  `location = /generate_204 {return 204;}`  即可
配置完毕直接重启 nginx，不报错就行。同样支持 https 页面。

本例对应的访问地址为：http://your.domain.name/generate\_204

server {
    listen   80;
    listen   443 ssl http2;
    server\_name  your.domain.name;
    location = /generate\_204 {
    return   204;
    }
}

---

### 2.2、测试 Nginx 回复的状态码

在这里我们使用 curl 命令即可测试，注意添加`-i` 参数，`-l` 参数由于返回 204 所以没有任何显示

curl \-i http://your.domain.name/generate\_204
*HTTP/1.1 204 No Content………………*
curl \-i https://your.domain.name/generate\_204
*HTTP/1.1 204 No Content………………*

当正确的显示出 HTTP/1.1 204 No Content 就可以进行下一步了。

---

### 2.3、使用 ADB 链接手机并配置

在这里首先你需要解压缩 ADB 的客户端，然后以管理员打开 Powershell，并进入 adb 根文件夹

然后我们用 usb 线连接手机，并在手机上开启 Adnroid Debugging，并确认 USB 指纹

接着在 Powershell 中输入 adb shell，如果正确连接时将直接进入一个新的命令行，成功后退出此命令行输入以下两条命令（Android 7.1.1 以上版本）

adb shell "settings put global captive\_portal\_http\_url http://your.domain.name/generate\_204";
adb shell "settings put global captive\_portal\_https\_url https://your.domain.name/generate\_204";

配置完毕后断开 ADB，重新链接 WIFI，过一会就可以看到叹号消失了。

---

## 3、完成体验

从根本上解决了叹号消失，以及自己的服务器响应在国内至少比 Google 快。

Android 默认的 Google Captive Portal 检测服务器是 //clients1.google.com/generate\_204
中国用户可以使用的 Google Captive Portal  检测服务器是 //g.cn/generate\_204

顺便大家可以用本站的一台开放 Captive Portal 检测服务器，开了不记录日志所以请放心。

adb shell "settings put global captive\_portal\_http\_url http://www.starryvoid.com/generate\_204";
adb shell "settings put global captive\_portal\_https\_url https://www.starryvoid.com/generate\_204";

---

## 4、相关链接

Wiki 介绍 Captive\_portal \[[链接](https://en.wikipedia.org/wiki/Captive_portal)\]
V2ex 的 Captive\_portal \[[链接](https://www.v2ex.com/t/303889)\]
关于 ANDROID 5.0\-7.1.2 网络图标上的感叹号及其解决办法 \[[链接](https://www.noisyfox.io/android-captive-portal.html)\]

