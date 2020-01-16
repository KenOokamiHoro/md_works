# 自由化 Android 的一点尝试

> 其实应该先发在咱自己的博客上的，但是发现装有咱 GitLab 私钥的笔记本已经让咱快递寄到家里去了……

## 其实几乎是个不可能的任务

因为 Android 自己就不怎么干净（？），哪怕是开放源代码的 Android Open Source Project，也有不少私有成分（例如 Linux 内核里到处乱飞（？）的厂商 Blob （一堆没有源代码只有二进制文件的部份，通常是固件和微码什么的））。

真正算得上完全自由的 Android 发行版大概只有 [Replicant](http://replicant.us/) 了，虽然到现在只支持到 Android 6.0 ，支持的设备也有限（最新的大概也只是 Galaxy S3/ Note 2那个时期的），已经有构建的设备的支持也很是问题（不少缺 WiFi 固件于是不能用 WiFi，图形性能也很糟糕……）。只能说任重而道远啊……

于是只能妥协一般的退而求其次，选一个差不多的系统就好了……

## 所以咱用手机都干些什么？

大概就这么几样（🤔）

* 电话和短信
* 拍照（基本上都是乱拍，或者是游戏的成绩图）
* 聊天（Telegram 居多，偶尔也用用 IRC 和 XMPP）
* 读电子书（？）和邮件（？？）
* 听音乐和视频（视频还可以听的？）
* 上各种网站（例如 Twitter、Mastodon、Matters 等等……）
* ……

## 试验环境（？）

就是咱要用哪一只手机来这么<s>瞎折腾</s>啦，介于各种原因以至于一定要把预装的系统换掉的缘故，可以方便的解锁和刷机的手机应该是不错的选择。咱就用咱手边的一加3了……

>当然要是买了 [Librem 5](https://puri.sm/products/librem-5/) 或者 [PinePhone](https://www.pine64.org/pinephone/) 的话，当咱没说…… 汝应该比咱更清楚该怎么做。

> 另外，这不是给完全没有 Android 手机使用经验的家伙们准备的指南。里面会有不少假定，例如汝已经了解了一些基本概念，学会了怎么给手机解锁等等。如果遇到不懂的名词或操作啥的，就像之前一样先上网搜索一下咯~

## 准备工作 - 解锁，Recovery 和系统

首先就是解锁啦，现在的手机基本上都会要求汝先在开发者选项中打开 OEM 解锁这个选项。

（别说汝不知道怎么打开开发者选项）

然后通过组合键或者 adb 命令把手机启动到 bootloader 模式，依手机的不同要进行的操作也不同。

* 像 Pixel 和 OnePlus 这样的手机基本上在进入 bootloader 模式之后就可以通过 <code>fastboot flashing unlock</code> 这样的命令解锁啦，当然汝还需要在手机上确认一下。
* 还有不少品牌会要求汝先在他们的网站上申请一串解锁代码，然后将这串解锁代码通过某种方式发送到手机上。遇到这种方法的厂商的手机的话建议参考厂商的指南。
* 以及解锁一定会要求清空所有数据恢复出场设定，如果汝不是用的一只新手机的话，记得备份一下原有的数据。

* 解锁后的手机都会多出一些提示来表示这只手机已经解锁了，例如启动前的提示或者某些功能缺失什么的（😅……）

接下来就是替换掉原厂的 Recovery 啦，因为原厂的 Recovery 一般只会接受原厂自己的升级/刷机包。比较流行的第三方 Recovery 就是 [TWRP（Team Win Recovery Project）](https://twrp.me) 啦，可以上他们的网站获得支持的设备列表，下载到合适的文件，以及了解不同手机的刷写方法。

至于系统嘛，咱选的是 CyanogenMod 的精神续作 [LineageOS](https://lineageos.org/) 。[这里有官方支持的设备列表](https://wiki.lineageos.org/devices/) （要进入官方支持的列表的要求还是挺高的，汝也可以上和汝手机相关的论坛等场所（最著名的非 [xda-developers](https://forum.xda-developers.com/) 莫属）看看有没有同好做过非官方适配），汝也可以选择汝喜欢的系统（只要有人进行适配的话，或者汝可以自己来）。

> 有经验的玩家应该听说过 Xposed 和 Magisk ，它们都是在一定程度上对系统模块进行修改或者加强的方案。它们本身也都是自由的（虽然它们之上的模块不一定），有需要的话也可以考虑装上。

## 商店 - F-Droid 

虽然 Android 不一定需要一个应用商店来安装应用（汝总是可以直接下载 APK 来安装，或者通过 adb 安装应用，是呗），不过可以考虑安装一个 F-Droid，这里面只有自由软件。

> **F-Droid **是一个 [Android](https://zh.wikipedia.org/wiki/Android) 应用程序的软件资源库（或[应用商店](https://zh.wikipedia.org/wiki/应用商店)）；其功能类似于 [Google Play](https://zh.wikipedia.org/wiki/Google_Play) 商店，但只包含[自由及开放源代码软件](https://zh.wikipedia.org/wiki/自由及开放源代码软件)。应用可从 F-Droid 网站或直接从 F-Droid 客户端应用浏览及安装。
>
> -- [Wikipedia:F-Droid](https://zh.wikipedia.org/zh/F-Droid)

F-Droid 有一个可选的特权扩展，可以让 F-Droid 通过系统权限安装和更新应用（如果只使用系统权限安装或更新应用的话，设置中的“允许未知来源”选项是可以关闭的，一定程度可以提高安全性。）可以[下载 OTA 包通过 Recovery 安装](https://f-droid.org/en/packages/org.fdroid.fdroid.privileged/) ，或者通过 [NanoDroid](https://forum.xda-developers.com/apps/magisk/module-nanomod-5-0-20170405-microg-t3584928) 或 [Magisk 仓库的一个模块](https://forum.xda-developers.com/apps/magisk/module-f-droid-privileged-extension-t3587068) 来安装。

然而虽然应用是自由的，但是它依赖的服务和扩展等等不是自由的情况也时有发生（例如 Telegram 的官方客户端有私有组建，服务端还是私有的）。这种应用在 F-Droid 里被称作“anti-feature”（叫什么好呢，“反功能”还是“负特性”？），默认情况下不会在列表里显示。汝可以在设置中的应用兼容性选项中打开“显示需要 anti-feature 的应用”选项来显示那些包含 anti-feature 的程序。

（例如 Telegram 的 anti-feature 列表， F-Droid 有一页收集了各种 anti-feature 和具有它们的应用列表： https://f-droid.org/wiki/page/AntiFeatures ）

## 各种应用的（部份）自由替代品

> 照着上面的需求一个一个挑咯……

* 电话、短信和拍照的话，用系统自带的大概就够用了。（LineageOS 内置的应该也是自由的吧……）
* 聊天的话， Telegram 有部份自由处理过的 [Telegram-FOSS](https://f-droid.org/en/packages/org.telegram.messenger/) ，XMPP 的话有 Xabber 和 Conversations （为啥咱会用两个呢，因为前者支持 XMPP 中最常见的端到端加密协议 OTR，另一个支持比较新的 OMEMO 和 openPGP…… （其实 Conversations 一开始也是支持 OTR 的，就是行为有些奇怪而且后来砍掉了……）😂）。IRC 的话就用 [WeeChat for Android](https://f-droid.org/en/packages/com.ubergeek42.WeechatAndroid/) 连上咱 VPS 上的 WeeChat 好了（别和某微幕搞混！）， Matrix 和 Mattermost 的话也都各自有客户端（ [Riot.im](https://f-droid.org/en/packages/im.vector.alpha) 和 [Mattermost](https://f-droid.org/en/packages/com.mattermost.rnbeta) ）。
* 浏览器的话， F-Droid 上有重新编译过的 Firefox（叫做 [Fennic](https://f-droid.org/en/packages/org.mozilla.fennec_fdroid/) ）， Guardian Project 也有 Tor Browser，[在这里添加它们的仓库到 F-Droid](https://guardianproject.info/fdroid/) 。
* 文件管理器的话，咱比较中意和习惯 [Amaze](https://f-droid.org/zh_Hans/packages/com.amaze.filemanager/) 。
* 网盘的话，咱有自己的 Nextcloud 实例，[所以当然就用它们的客户端啦……](https://f-droid.org/zh_Hans/packages/com.nextcloud.client/) ， F-Droid 上也有不少可以和 Nextcloud 协作的应用，例如 [RSS 阅读器 Nextcloud News Reader](https://f-droid.org/zh_Hans/packages/de.luhmer.owncloudnewsreader/) 和 [笔记辅助软件 Notes](https://f-droid.org/zh_Hans/packages/it.niedermann.owncloud.notes) 。

* 一时兴起想要写些什么的话，除了 Notes 以外也可以试试 [Markor](https://f-droid.org/en/packages/net.gsantner.markor/) 。
* 想和以前一样读点什么的话，咱就用 [FBReader](https://f-droid.org/en/packages/org.geometerplus.zlibrary.ui.android/) 好了，虽然有些插件不是自由的……
* 当然不能忘了关键的输入法啦，咱们有基于 Rime 的 [TRIME](https://f-droid.org/en/packages/com.osfans.trime/) 可以用。
* 想在 Android 上运行一些 GNU/Linux 工具，例如 ssh 之类的话，可以试试 [Termux](https://f-droid.org/en/packages/com.termux/) ，也有一些像是浮动窗口或者快速运行脚本的扩展用。觉得虚拟键盘操作不方便的话可以尝试有方向键和功能键的 [Hacker's Keyboard](https://f-droid.org/en/packages/org.pocketworkstation.pckeyboard/) 。
* 阅读和发送电子邮件的话，咱比较喜欢老成（？）的 [K-9 Mail](https://f-droid.org/en/packages/com.fsck.k9) ，也有比较新潮的 FairMail 啥的可以考虑，它们都能和另一个 openPGP 应用 [OpenKeyChain](https://f-droid.org/en/packages/org.sufficientlysecure.keychain/) 一起使用来发送和解密 GPG 加密的邮件。

----

## 自由路漫漫

既然选择了这条路，就一定会付出一些代价，例如 Google 服务几乎全都没法用了什么的（虽然有自由的客户端实现 [microG](https://microg.org/) 在努力解决这个问题）。以及汝自己忙活了那么久发现身边的朋友完全不在意甚至报以狐疑的目光什么的……

## 相关阅读（？）

写到最后突然想起咱之前写过一篇 [离开 Google 的 Android 之路](https://blog.yoitsu.moe/tech_misc/android_without_google_0.html) ，虽然那个主要是写 Android 去 Google 化的，也拉过来当作参考吧……

> 去 Google 化？某强国不是已经完成了嘛？（咋不说跳进了更大的坑呢……）



