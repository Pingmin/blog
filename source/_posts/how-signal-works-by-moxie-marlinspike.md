---
title: "當世界向前，回顧暗號是如何做的（Looking back at how Signal works, as the world moves forward）"
author: 馬克希·馬琳斯巴克(Moxie Marlinspike)
date: 2020-06-12 23:13:40 +0800
updated: 2020-11-12 21:30:50 +0800
tags:
- 安全(Security)
- 端到端加密(End-to-End Ecryption, E2EE)
- 馬克希·馬琳斯巴克(Moxie Marlinspike)
- 暗號(Signal Messenger)
- 推荐(Recommended)
- 翻譯(Translation)
categories:
- 信息技术(IT)
---

![Signal Logo](https://signal.org/assets/header/logo-f7ef605fe417d5520d38d546b3b774b4261c75220b9904da4d8b2ffc19a761ff.png)

## Looking back at how Signal works, as the world moves forward（當世界向前，回顧暗號是如何做的）

> <img src="https://moxie.org/images/moxiemarlinspike2.jpg" width="30%" height="30%" alt="馬克希·馬琳斯巴克(Moxie Marlinspike)">
>
> （英文作者：[馬克希·馬琳斯巴克(Moxie Marlinspike)](https://github.com/moxie0)（譯注：Signal 和開放密語系統的創始人，如下圖），[英文原文](https://signal.org/blog/looking-back-as-the-world-moves-forward/)，20200605）
>
> （中文譯者：[平民（Pingmin Fenlly Liu）](https://pingmin.me)，20200609-0612）


In the midst of world-wide protests against racism and police brutality, a lot of people are becoming more immediately aware and concerned about the security of their data and online communication. We’ve gotten a lot of questions at Signal over the past week, so we wanted to briefly recap how it is that we’ve designed Signal, and how we think about concepts like privacy, security, and trust.
在波及全球的針對種族歧視和警察暴行的抗爭中，許許多多的人們正立刻變得更加有意識地去關注他們數據與在線通訊的安全性。在過去的一周里，我們收到了很多關於“Signal（暗號：秘密聊天工具）”的問詢。所以，我們想簡單地摡述下我們是如何設計 Signal 的，以及我們是如何思考諸如隱私、安全和信任等概念的。

<!-- more -->

What if the worst should happen, and some unauthorized party were to compromise Signal? We don’t have to speak hypothetically, because the US government already tried this, so we can examine what that looked like. In 2016, the US government obtained access to Signal user data through a grand jury subpoena from the Eastern District of Virginia. However, there wasn’t (and still isn’t) really anything to obtain. At the time, [we worked with the ACLU](https://www.aclu.org/blog/free-future/new-documents-reveal-government-effort-impose-secrecy-encryption-company) to fight the gag order that was intended to prevent us from publishing this information, so you can [see the full subpoena and response here](https://signal.org/bigbrother/eastern-virginia-grand-jury/).
要是發生了最壞情況，以及一些未授權方想違背或損害 Signal（的隱私保護等相關原則/價值觀），結果會怎樣？我們無須假設，因為美國政府已經如此嘗試過。2016 年，美國政府（執法部門）通過弗吉尼亞東區法院一大陪審團傳票（司法程序）的授權得以獲取相關 Signal 用戶的數據。然而，我們這（服務器上）當時（至今也是）實際上并沒有什麼數據可取到的。那時，[我們也與美國公民自由權聯盟（ACLU）一起](https://www.aclu.org/blog/free-future/new-documents-reveal-government-effort-impose-secrecy-encryption-company)，與意欲阻止我們公開這些信息的壓制言論自由的政令做抗爭，你可以在這裏[查看完整的傳票及其回應的內容](https://signal.org/bigbrother/eastern-virginia-grand-jury/)。

The only Signal user data we have, and the only data the US government obtained as a result, was the date of account creation and the date of last use – not user messages, groups, contacts, profile information, or anything else.
我們所擁有的 Signal 用戶的唯一數據，也是美國政府最終所獲得的唯一數據，是賬號創建日期和最後使用日期——沒有用戶的消息、群組、聯系人、個人資料等其他任何信息（見下圖）。

![Signal user data（用戶數據）](https://signal.org/blog/images/subpoena-screenshot.jpg)

This is because we’ve designed Signal to keep your data in your hands rather than ours. Signal uses end-to-end encryption so that we never have access to the contents of the messages you send; they are only visible to you and the intended recipients. However, Signal also applies this design philosophy to the rest of your data as well.
這是因為我們把 Signal 設計成將你的數據保存在你的手中而不是我們手中。Signal 采用了端到端加密（E2EE）技術，這樣即使是我們也無法訪問你所發的消息明文（因已加密且第三方幾乎無法解密），它們只能被你自己和消息的接收者所看到。同時， Signal 也將此設計哲學應用在了你的其他數據上。


### Our approach（我們的方法）

Unlike any other popular messaging apps, Signal also does not have access to your contacts, social graph, group data, group membership, profile name, profile avatar, location data, gif searches, etc. – and we don’t include trackers, ads, or analytics in our software at all.
與其他任何的流行通訊應用不同，Signal 也無法訪問你的聯系人、社交關系圖譜、群組數據、群組成員、個人名稱、個人頭像、定位數據、GIF 搜索數據，等等——我們在我們的軟件中也一點都不會進行數據追蹤、廣告展示和統計分析。

Because we’ve built Signal to completely avoid storing any sensitive information, I can stand on stage in front of thousands of people and [publish all of my account data](https://youtu.be/Nj3YFprqAr8?t=894) publicly without revealing anything other than how long I’ve had Signal installed (it’s since I last replaced my phone) and the last date I had Signal installed (it’s today btw).
因為我們在做 Signal 時，就是完全避免存儲任何敏感信息的，我可以面對幾千人站在舞台上公開地[發布我所有的個人賬戶數據](https://youtu.be/Nj3YFprqAr8?t=894)（譯注：YouTube 視頻，內地需科學上網才能查看），包括我安裝 Signal 多久了（因為我後來換了手機）以及我最後的安裝日期（也就是今天）。

If you ask the CEO of any other major communication platform to publicly publish their account data from their platform, they won’t.
如果你讓任何其他主要通信平臺的首席執行官（CEO，類似于我們的總經理），公開他們在他們平臺上自己的賬號數據的話，他們是不會公開的。

I don’t blame them – it’s a lot of data that they would probably be uncomfortable sharing with you – but it raises the question of whether we should be comfortable sharing the same data with them.
我并不是在指責他們——那會有很多數據如果分享給你會引起他們不適——而這便引發了一個問題：難道我們的數據（因存放在他們服務器上而）分享給了他們，我們就會舒適嗎？


### Sync different（不一樣的同步）

This represents a fundamental difference in how we think about concepts like privacy, security, and trust. We do not believe that security and privacy are about “responsibly” managing your data under our control, but rather about keeping your data out of anyone else’s hands – including our own.
這體現了我們在如何思考諸如隱私權、安全性和信任等觀念上的根本不同。我們相信，安全性與隱私權，不是如何在我們的控制下負責任地管理你們的數據，而是讓你們的數據不落入任何其他人的手中——包括我們的。

We believe this because, even after 30 years of trying, anything else has proven to be a losing strategy. Data breach after data breach, and the incentives that have emerged from the monetary value of data, have led to a dramatic loss of privacy online, especially for some of the most intimate conversations we have.
我們之所以如此相信，是因為，即使已經嘗試了三十年，任何其他的做法都已被證明是失敗的策略。一次次數據洩漏，以及所暴露出來的在數據的金錢價值上的誘惑，已經導致了互聯網線上隱私的戲劇性的巨大損失，尤其是在一些我們最私密的對話上。

We don’t believe that trust is about trusting us with your data, but rather about trusting our engineering abilities and know-how to design software that keeps your data in your hands rather than ours or anyone else’s. In order to help build that trust, we’ve made [all of our software open source](https://github.com/signalapp/) so anyone can look at how we design and build things. There are no secrets in there, because we never have access to your secrets to begin with.
我們相信，信任不是信任我們如何保有和使用你們的數據，而是信任我們的工程能力以及知道如何設計軟體以將你們的數據保留在你們自己手中而非我們或任何其他方手中。為了幫助建立此信任，我們已經[開源了我們所有的軟件](https://github.com/signalapp/)，這樣任何人都可以直接通過軟件源碼來查看我們是如何設計和構建這些的。那裡沒有秘密，因為我們從來沒接觸到你們的秘密及以其開展軟件系統的開發等。

We also make this technology publicly available for free because Signal is a [501(c)(3) nonprofit](https://signal.org/donate/). Our mission is to increase privacy online, so we publish our technology and share knowledge to encourage other companies to adopt it in their own products and services.
我們也公開了這項端到端加密技術（即暗號協議（Signal Protocol）并使公眾可自由免費使用，因為暗號 （Signal）是一個在美國注冊成立的[“501(c)3”型的非營利組織](https://signal.org/donate/)（即暗號基金會（Signal Foundation））。我們的使命是增強在線隱私權，所以我們公開我們的技術和分享這些知識，以鼓勵其他公司在他們的產品和服務中採用這些技術。

### Moving forward with you（與你一同向前）

Every feature that we add to Signal is a new opportunity to make sure that your information is only accessible to you. This isn’t how most organizations handle user data, but it has always felt like the right approach to us. These are your contacts, your conversations, your photos, and your information – not ours – and it’s your powerful voices that are out there organizing and advocating for change.
暗號（Signal）所添加的每一個功能，都是一個確保使你的信息僅被你所訪問的新機會。雖然大部分組織和公司都不是這麼處理用戶數據的，但是對於我們來說，這始終感覺才像是正確的方式。這些都是你的聯系人、對話、照片及信息——不是我們的——它們也是你組織和提倡變革的強有力的聲音。

Keep on sending a message, and we’ll keep making sure they get delivered securely.
你們繼續發送消息，我們也將繼續會確保安全地傳送它們。

（完（END））


## 譯注（Notes）

（1）首先需要聲明的是，平民與 Signal 沒有商業利益關係，于此翻譯和分享僅作為一種價值上的認可和推薦。

（2）Signal Messenger 是平民首選的安全即時通訊工具。另，據 @李仲偉 律師所說，律師也基本都用 Signal。因為 Signal 是以手機號為賬號（服務器上保存的實則是截取了手機號哈希值的一部分作為賬號識別 ID）的，所以使用 Signal 的我們，通常會優先留存聯系人的手機號，因為我們都互相很信任，同時也我們也會優先考慮公開自己的手機號作為聯繫方式，因為使用Signal并具有相同價值取向的我們一看就懂。

（3）這次翻譯，是平民自 2007 年首次嘗試翻譯喬布斯 2005 年在斯坦福大學的演講稿以來的第二次翻譯嘗試，所以目前只是以作為技術人員的職業視角來翻譯的，沒有刻意去追求翻譯的“信、達、雅＂要求。同時，也歡迎指教！

（4）感興趣或同樣注重通訊安全的話，可以試下 Signal（可隨時注銷，不留痕跡）：

> [Signal - Private Messenger（暗號 - 私密聊天工具）](https://signal.org)
>
> [Signal for iOS](https://itunes.apple.com/cn/app/signal-private-messenger/id874139669)
>
> [Signal for Android](https://signal.org/android/apk/) (如：點此下載[當前最新版(v4.76.3)的 apk](https://updates.signal.org/android/Signal-Android-website-prod-universal-release-4.76.3.apk))
>
> 更多可參考此個人筆記：[Favorites（收藏）](https://fenlly.org/favorites)。
>
> 另，對於安裝與使用上的問題，需要的話，你可以在公眾號後臺留言，平民自願免費提供技術支持！


## 印记(Imprints)

（1）20200609-0612：利用空闲时间（在地鐵二號線及唐鎮住所）完成譯文， 20200612 首次公開於公衆號"风灵学园FENLLY"。
（2）20201112：整理和完善文本，調整排版，同步發布到此[個人博客](https://pingmin.blog)。
（Next）xxxx：更新了xxx。
（Todo）待做：同步英文翻译。


平民（Pingmin Fenlly Liu）
20200609-1112
