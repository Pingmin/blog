---
title: 安全即时通讯(Secure IM)
date: 2017-08-29 20:29:36 +0800
updated: 2018-03-11 20:28:26 +0800
tags:
- 安全(Security)
categories:
- 信息技术(IT)
---

## 概要(Abstract)

本文主要说明了“端到端加密（End-to-End Ecryption, E2EE）”的基础知识及其相关的即时通讯（Instant Messaging, IM）应用（App）。

## 正文(Body)

先来看下我国当前相关的法律条文：

<!-- more -->

> 1. 《**中华人民共和国宪法**》第四十条
>
>    中华人民共和国公民的通信自由和通信秘密受法律的保护。除因国家安全或者追查刑事犯罪的需要，由公安机关或者检察机关依照法律规定的程序对通信进行检查外，任何组织或者个人不得以任何理由侵犯公民的通信自由和通信秘密。
>
> 2. 《**中华人民共和国刑法**》第二百五十三条之一
>
>    【侵犯公民个人信息罪】
>
>    违反国家有关规定，向他人出售或者提供公民个人信息，情节严重的，处三年以下有期徒刑或者拘役，并处或者单处罚金；情节特别严重的，处三年以上七年以下有期徒刑，并处罚金。
>
>    违反国家有关规定，将在履行职责或者提供服务过程中获得的公民个人信息，出售或者提供给他人的，依照前款的规定从重处罚。
>
>    窃取或者以其他方法非法获取公民个人信息的，依照第一款的规定处罚。
>
>    单位犯前三款罪的，对单位判处罚金，并对其直接负责的主管人员和其他直接责任人员，依照各该款的规定处罚。
>
> 3. 《**中华人民共和国网络安全法**》第四十一条
>
>    网络运营者收集、使用个人信息，应当遵循合法、正当、必要的原则，公开收集、使用规则，明示收集、使用信息的目的、方式和范围，并经被收集者同意。
>
>    网络运营者不得收集与其提供的服务无关的个人信息，不得违反法律、行政法规的规定和双方的约定收集、使用个人信息，并应当依照法律、行政法规的规定和与用户的约定，处理其保存的个人信息。



而在通讯中，对通讯内容进行＂端到端加密（End-to-End Encryption, E2EE）＂，可最大限度地防止除通讯双方以外的第三方（包括应用程序本身的运营方）窃取通讯內容。故推荐使用支持＂端到端加密＂的通讯应用进行通讯（包括含文字、语音、文件和音视频通话等），中华内地目前推荐一个叫“Signal（暗号）”的开源应用（详见文末的附录部分）。

目前，除了 Signal ，此类安全应用还有 WhatsApp 、Telegram、Bleep、Line 、Apple iMessage/FaceTime、Google Allo 、Facebook Messenger 、超信 和 安司密信（安司密信另有企业、政务和军采版）等。不幸的是，前面提到的大部分应用，都受到了“防火长城（GFW）”等的影响，导致有时无法正常登录使用，且有的已经完全被屏蔽和下架了。

不过，目前 Signal 还可以正常使用，且支持 iOS、Android 以及三大桌面系统（Windows、macOS、Debian-based Linux）（参见下面附录部分）。而 WhatsApp 平常一般都能正常使用，也支持多个系统平台，只是偶尔（特殊时期）会受到一点干扰。所以，现在个人用 Signal 比较多，而之前则用 WhatsApp 要多一些（始于 2016 仲夏或更早）。当然，鉴于我们当局的惯用做法，不排除他们将来会把 Signal 和 WhatsApp 等也都完全屏蔽的可能性。

温馨提示一下，如果你用的是苹果公司的产品的话，如 iPhone、iPad 和 Mac 等，你也可以使用 iMessage 和 FaceTime 进行安全通讯，只不过需要通讯双方都使用苹果的设备（注：网上也有人开发了相应的 App 以使 Android 也可以与苹果设备发送 iMessage 消息）。

值得一提的是，开源应用 Signal 更加注重对隐私的保护。前美国中央情报局（CIA）技术分析员、美国国家安全局的＂棱镜＂监听项目（参见著名的＂棱镜门＂事件）的泄密者“爱德华·斯诺登”也对 Signal 做了推荐（参见 Signal 官网）。


### 附：

#### 一、Signal（暗号）

1. 官方网站： https://signal.org/ 。

2. 开源项目： https://github.com/signalapp/ 。

3. Android 版：下载页面为 https://www.signal.org/android/apk ，目前最新版为 https://updates.signal.org/android/Signal-website-release-4.15.5.apk （手机上请复制粘贴到浏览器中打开，以下载安装）。

4. iOS 版： https://itunes.apple.com/us/app/signal-private-messenger/id874139669?mt=8 。

5. macOS 版： https://updates.signal.org/desktop/signal-desktop-mac-1.5.2.zip 。

6. Windows 版： https://updates.signal.org/desktop/signal-desktop-win-1.5.2.exe 。

7. Linux (Debian-based) 版：
    （1）直接下载： https://updates.signal.org/desktop/apt/pool/main/s/signal-desktop/signal-desktop_1.5.2_amd64.deb

    （2）或通过添加官方仓库来安装（以后可随系统自动更新）：
    ``` bash
        $ curl -s https://updates.signal.org/desktop/apt/keys.asc | sudo apt-key add -
        $ echo "deb [arch=amd64] https://updates.signal.org/desktop/apt xenial main" | sudo tee -a /etc/apt/sources.list.d/signal-xenial.list
        $ sudo apt update && sudo apt install signal-desktop
    ```


#### 二、安全性说明（Signal 与 WhatsApp 等都类似）

参见：
（1）https://www.whatsapp.com/security/ 。
（2）https://support.signal.org/ （目前是全英文）。

#### 三、WhatsApp

官方网站： https://www.whatsapp.com/ 。
Android 版： https://www.whatsapp.com/android/current/WhatsApp.apk 。
iOS 版： http://itunes.apple.com/us/app/whatsapp-messenger/id310633997?mt=8 。
其他版本可在其官网下载： https://www.whatsapp.com/download/ 。

#### 四、备用下载

百度网盘（含 Signal 与 WhatsApp）： https://pan.baidu.com/s/1miFtFxI 。

## 印记(Imprint)

（1）20170829：首次作为手稿和内部消息（《推荐使用更安全的手机通讯应用（含简洁版和详细版）》）公开于部分微信好友群聊中。
（2）20170901：简化并增加部分英文翻译，并首次作为“评论”公开于个人最近的一条微信朋友圈动态下。
（3）20170905-1012：进行了多次修改、更新和完善。
（4）20171119：增加国内支持端到端加密的部分通讯应用，以及相关法律条文。
（5）20180107：重新排版和组织并更新相关内容。
（6）20180311：（a）增加另外两个新的法条并优化排版，并完善相关正文内容；（b）更新 Signal 和 WhatsApp 官方的相关下载连接；（c）修正备用的百度网盘下载链接。
（N）下一步（Next）: xxx。
（T）待做（TODO）：同步英文翻译。

## 署名(Attribution)

如果转载，请至少保证“正文”部分的完整性且至少须以链接形式给出的原文链接。
具体请参见“[指南(Guides)](/guides)”页面中与版权相关的CC授权协议及注意事项等。请不要构成侵权行为，谢谢合作。



刘水良(Pingmin Fenlly Liu)

20170829-20180311

## 结束(End)
