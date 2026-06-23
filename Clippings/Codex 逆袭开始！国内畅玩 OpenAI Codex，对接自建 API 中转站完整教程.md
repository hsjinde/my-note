---
title: "Codex 逆袭开始！国内畅玩 OpenAI Codex，对接自建 API 中转站完整教程"
source: "https://www.youtube.com/watch?v=t83I-yEgTU8&t=476s"
author:
  - "[[科技lion]]"
published: 2026-05-10
created: 2026-06-23
description: "图文教程https://blog.kejilion.pro/openai-codex-api-gateway/莱卡云VPS优惠https://www.lcayun.com/aff/ZEXUQBIM更多VPS优惠推荐https://kejilion.pro/index.php/topVPS/节目中使用的SSH工具：Termiushttps://app.kejilion.pro/term"
tags:
  - "clippings"
---
![](https://www.youtube.com/watch?v=t83I-yEgTU8)

图文教程  
https://blog.kejilion.pro/openai-codex-api-gateway/  
莱卡云VPS优惠  
https://www.lcayun.com/aff/ZEXUQBIM  
更多VPS优惠推荐  
https://kejilion.pro/index.php/topVPS/  
节目中使用的SSH工具：Termius  
https://app.kejilion.pro/termius/zhanzhang/  
  
科技lion官方社区，学习交流新基地  
https://bbs.kejilion.pro/  
科技lion官网正式上线！科技博主的自我介绍，欢迎光临  
https://kejilion.pro/  
LION导航上线了，打造专业的云服务导航，欢迎收藏  
https://dh.kejilion.pro/  
个人博客，记录更多科技白嫖生活，欢迎光临  
https://www.kejilion.xyz/  
频道官方群，欢迎加入  
https://t.me/joinchat/Tx0ycqPTv3mMcyoD  
甲骨文云白嫖群，欢迎加入  
https://t.me/+mXXJBhNvV6E0YWM1  
亚马逊云白嫖群，欢迎加入  
https://t.me/lionAWS  
频道会员，更多精彩内容，欢迎加入  
https://www.youtube.com/channel/UCKqRheBWWCATRulqT1vxrww/join  
  
  
  
章节：  
00:00 开场：主流 AI 智能体使用情况调查  
00:13 OpenClaw、Claude、Hami、Codex 等工具对比  
00:39 Codex 近期势头与国内使用优势  
00:57 ChatGPT / Codex 的代码与多模态能力  
01:51 Codex 官网与 Windows 客户端下载安装  
02:12 安装后体验：让 Codex 查看本地磁盘空间  
03:23 官方账号额度、Plus 订阅与五小时重置机制  
03:46 Codex 桌面端交互体验与图片上传能力  
04:04 Codex 读取本地电脑内容的实际演示  
04:35 指定项目目录，让 Codex 聚焦处理本地工程  
05:18 Codex 分析项目技术栈与功能模块  
06:24 为什么要对接第三方 API / 自建中转站  
06:39 CCProxy API / Super2 API 中转服务思路  
07:03 使用莱卡云服务器搭建演示环境  
07:30 运行科技Lion一键脚本安装 CCProxy API  
08:14 设置后台登录密码并进入管理后台  
08:33 为什么推荐用域名和 HTTPS 访问中转站  
08:44 Cloudflare 域名接管与 DNS 解析说明  
09:25 添加 A 记录并关闭橙色云代理  
10:14 在脚本中绑定域名并自动安装 Nginx / 申请证书  
11:01 域名访问后台与登录注意事项  
11:30 清理默认配置并生成新的 API Key  
11:49 添加 Codex / Claude Code / Gemini CLI 等账号  
12:39 查看账号额度并准备对接本地 Codex  
12:51 Windows 中找到 Codex 配置文件与 Key 文件  
13:10 修改 API 地址、模型与 Key 配置  
13:32 覆盖配置文件并重新启动 Codex  
13:51 验证 Codex 已接入自建 API 中转站  
14:26 中转站后台查看当前调用状态  
14:59 总结：官方与自建中转站两种使用方式对比  
15:21 结尾：配置文件与相关内容获取方式

## Transcript

### Intro: Survey of Popular AI Agents

**0:00** · 哈喽大家好 欢迎来到今天的这期节目 我做了一个基本的调查 就是大家经常使用的 ai智能体 是什么 有很多小伙伴都是一起在用的 我们可以看到 这些目前都是比较主流的ai智能体 包括OpenClaw、Claude 还有Hami、Codex、Claude Code这四个

### Comparing OpenClaw, Claude, Hami, Codex, and Other Tools

**0:21** · 从数据上来看到 我们开源生态下的OpenClaw呢 还是占据比较大的一部分 也有可能是根据我频道的一个成分 来决定的 然后我们可以看到闭源的Claude coder这边占比还是比较高 然后Codex 目前我看这个势头会比较好 而且有很多小伙伴都转战 因为它对于国内来讲会比较友好一点 然后早期的话 我们是用的命令行 CLI 的方式

### Why Codex Is Gaining Momentum, Especially for Users in China

**0:53** · 使用这个Codex 其实目前ChatGPT呢 不光是解代码能力很强 包括我们文生图的能力也是非常强的 目前gpt的账号溢价特别高 然后价值感拉满 什么含金量还在不断的上升 gpt在多模态层面上 已经是碾压趋势了 是自从这个GPT-Image-2出来以后呢

### ChatGPT / Codex Coding and Multimodal Capabilities

**1:20** · gpt的这个账号就疯抢了 因为很多小白呢 简单的说一些提示词 就能出大片级的图片 很震撼的 龙虾的作者不是也到那个openai去了吗 Codex呢完成度那就是非常高的

**1:38** · 就是商业级的水准 就不像龙虾那么不稳定 当然它也会有一定的稳定性 那也完全取决于 网络层面上的一些稳定性 然后这个呢 就是Codex的一个官网 然后你可以直接下载windows版本 windows版本目前做的也还是可以的 当然没有Mac那么原声 你们你直接点击这个进行下载 然后下载好以后呢 就是这样的一个图标 你直接可以双击进行一个安装 它走的是windows自带的商店

### Codex Official Website and Windows Client Installation

**2:10** · 装过了就叫更新 然后你可以看到 我用这个gpt生成了很多很多图片 安装好以后他就基本上可以使用了 你现在跟他说的是 可以让他帮我看一下z盘的空间 先先试一下他能不能行 这个是非常基础的 你只要登录你的gpt的账号 你就可以去这样的去使用它 现在是全账号段都是可以用Codex的

### First Test: Let Codex Check Local Disk Space

**2:37** · 不像那个Claude 它必须是订阅用户才行 那我建议大家使用一个plus 这样的一个订阅 就可以去使用Codex 并且很充裕 个人来讲 使用量是没有什么太大问题的 当然如果你的任务量会比较重 可能五个小时的重置期间 你就可能已经把你的用量用完了 你要等五个小时以后 才能再接着去使用 很多大厂目前都是五个小时重置的

**3:09** · 这样的一个思路已经成为行业标准了 所以你登录自己账号也行 如果你觉得我有多个账号 我想做中转站 那你当然也可以配置中转站的信息 我可以跟大家分享怎么去配置 如果你走过官方 你这边能看到你剩余量是什么样的 他这边也讲的比较细 五小时一周这样的一个用量还有多少

### Official Account Quotas, Plus Subscription, and 5-Hour Reset Cycle

**3:35** · 如果你发现各种超时呢 我们尝试重新登录一下 他现在已经开始帮我们去查了 他这种交互体验还是非常好的 比起 CLI终端上要显示的更好 而且他这边还可以支持上传图片 他就帮我找到了 我现在已经用了600多 G，还剩 200G 这些都是比较简单的 我们可以对照一下 可以看到他的一个判断是没有问题的 他真正意义上 是能到你电脑上去看东西的

### Codex Desktop Experience and Image Upload Support

### How Codex Reads and Works with Local Computer Files

**4:04** · 还有一种做法 你可以框定它在某一个目录文件夹 比方说我有一些工程 我比方说有一个科技Lion Panel 我把这个锁定进来 它就相当于在这个目录下 去创建一个新的对话 我问一下这个是什么项目 我一般的操作都是这样的 其他的你就默认这边可以调gpt

**4:27** · 5.5目前是最高的 你也可以调成超高 智能性会更高一点 我们这边点击 让他去确认一下 我问一下这是个什么项目 看他能不能帮我们 把本地的项目去处理 当然 他不会像gpt网页版那种那么迅速

### Limit Codex to a Specific Project Folder

**4:47** · 因为它所有的内容都是思考的 这个过程 都是要在你本机进行一些操作的 去确认的 所以它的速度会会很慢 这也就是智能体 为什么跟网页版的这 这种纯聊天的不一样 他是真正能把控你电脑内容的 当然可能还是会有一些超时的问题 这个需要大家用一些其他的网络环境

**5:14** · 来去弥补 就是让网络变得更加稳定 判断他的一个技术栈 目前看到了一些HTML、CSS、JS 文件类型 他在过程当中会发现一些问题 他自己会解决 他会总结好以后就会跟我罗列 他说会有系统监控 DocKeyr管理 还有网站的管理 比较关键的一点 他没看到代码的API 服务 是因为我们当时没有接 专门针对React的Linux的一个部署

### Codex Analyzes Project Tech Stack and Features

**5:46** · 他这个管理是我目前看是比较清晰的 要比Lovable、Hami的那些要清晰 因为Lovable、Hami那篇它是消息渠道 就类似于Telegram、微信，还有企业微信 这些呈现上还是有一些欠缺的

**6:04** · 那如果你在本地用Codex 这个就非常方便了 对每个目录进行操作的时候 我就直接在这个目录项目下 进行一个对话就可以了 可以让他集中注意力去完成你的项目 而非它在全盘进行搜索 这样来的效率高 那接下来我来跟大家分享一下 对接你第三方API的具体步骤

### Why Connect Codex to a Third-Party API Gateway

**6:29** · 好现在你可以直接用这个gpt 把你的账号进行登录 但是这样的只能登录一个账号的额度 你用这个CCProxy API 或者用这个叫什么Super2 API中转服务 你都可以 就会包含我所有的这一类的模型

### CCProxy API / Super2 API Gateway Concept

**6:48** · 它甚至可以在Codex里头去使用 Claude的Opus 我们的gemini也都是可以去使用 可以看一下额度 我一个是团队号 一个是plus 那我们就可以让两个合并 我们需要先自己搭建一个 这个依然使用莱卡云的服务器 来给大家做演示吧 把这个服务先搭起来 你们可以到他的官网去看 他将香港 日本韩国主打的海外层面的 国内当然也会有很多 每个月这边都会有一个优惠活动

### Using a VPS to Build the Demo Environment

**7:21** · 你可以去看 ¥19.9.的优惠 就是我们搭建类似于中转 这样的一个服务 就可以绑定你多个扣ex的账号 我们需要运行科技Lion的一键脚本 科技Lion的脚本 你就记住这样的一个命令 这样的一个网站 你复制进来 它就会自动跳转到 你符合浏览器语言的这个网站 然后你复制一下这个代码 然后右键进来回车

### Installing CCProxy API with the KejiLion One-Click Script

**7:48** · 然后他就来到这样的一个页面 这样一页面我们选择11 选择CCProxy API 你就直接选择它 那么选择一 让它直接进行一个安装 默认端口 它就会去拉取官方的安装方式

**8:04** · 这边你输入一个登录密码 这个登录密码是不会显示的 输入完以后你就直接回车 这个密码你要记住 这样我们就可以去使用 去登录到它的后台 对我们用这个地址点击登录啊 首页 你把这个复制一下 这个是作者他故意为置的 他是分离的 那现在我们就来到这样的公网地址 其实你就可以作为a pi的url进行使用了

### Setting the Admin Password and Logging into the Dashboard

**8:31** · 当然我更建议大家去使用域名 因为http它没有证 输入相关的安全性可能会差一点 我们把刚才我们在那边输入的密码 输入进来 给它记住密码 点击这样它就登录到后台了 你要做反向代理找到你的Claudeflare 你买一个域名接管到这边来 Claudeflayor它就是一个域名接管商 并且带cdn的边缘加速的 个人玩家是非常推荐的 所以你要自己做中转站 就要比那个纯Codex那边要更进阶一点

### Why You Should Use a Domain and HTTPS

### Cloudflare Domain Management and DNS Setup

**9:01** · 更麻烦一点 但是你的用量就会更多一点 选择一个域名 我这边都已经绑定的 如果你没有绑定 你直接点这个添加去连接你的域名 你也可以直接在Claude flare这边注册域名 我们这边就直接找个xyz

**9:20** · 他现在这边改的已经面目全非了 我已经感觉不出来了 这边有个记录 记录里头我们把这个添加 我们添加一个a 记录就是IPv4的 这边我们给他一个什么 给他一个ci 对API这样 好吧最后的格式就是ciAPI 点xyz 地址我们就填莱卡云的地址 莱卡云的地址就是把它复制一下 贴过来把这个删掉 把后头的端口号也删掉 就纯ip地址这边关掉小云朵

### Adding an A Record and Disabling the Orange Cloud Proxy

**9:52** · 因为开小云朵如果大家不会设置 会导致你这个中转站用不了 回头我再跟大家分享 如何去把防火墙相关的去关掉 让开Claudeflayare的时候你也能去使用 ok 现在呢我们就点保存 那我们就解析了一个c API的这样的一个域名 到我们来看云的这台机器上 解析好以后 你直接这边输入五 把你刚才解析的那个域名粘贴过来

### Binding the Domain, Installing Nginx, and Applying for SSL

**10:19** · 好你可以看到这样的格式没有问题 直接回车就行 他就会去给你装Nginx 如果你没有 会去把这一套全都给你装上 你就可以用域名访问你这个中转站了 访问中转站用域名 好处就是安全带证书 所有互联网标准化 用ip你再加加证书 会不推荐不是最佳实践 好了我们Nginx已经装好 好了 现在他在拉取这个证书的

**10:50** · 容器来为我们申请证书 有小伙伴申请证书申请不下来 很有可能就是你的防火墙的问题 那我这边一路畅通 没有问题了 好了那这时候我们再点这个 我们发现 这个好像跟我们ip访问的时候是一样的 那没错了 反向代理成功了一样 也要加这个html

### Accessing the Dashboard via Domain Name

**11:10** · 这个地方要注意 这个地方一个斜杠 这边井号不要了 就直接这样的域名的方式 我们已经来到了我们的后台 依然保存我们的密码 我们输入刚才设置的密码 好这样我们就登录进来了 这是最佳的一个心态 我们就可以在这边我们再设置这块

### Cleaning Default Settings and Generating a New API Key

**11:30** · 你把这其他的都删掉 这些都没用的 点击这下头的保存 点击确认 我建议全删了 我们建议自己再创建一个 创建一个新的 生成一个新的Key 这个就是你到时候去用的

**11:45** · 你先保存一下 点击对勾 点击确认 通过这个登录 来去登录你的Codex 或者登录你的Claude Code的 Antigravity的啊 Gemini CLI的等等 这些你都可以去登录 常用的就是Codex 我们点击登录一下 我们这边输入一下 我们刚才的plus账号 这个是团队号 我们先加一下 好把这个地址复制出来 回掉到这边 回掉到这个里头 点击它就可以加进来了 好那我们添加了一个对不对

### Adding Codex / Claude Code / Gemini CLI Accounts

**12:16** · 我们再添加另外一个 好吧看一下 同样也会给我们一个反馈地址 我们再把它粘回来 好这样我们有两个了 我们我刚才添加的 也有可能是普通账号 才会遇到接码的情况 这个可以大家进一步反馈给我 我们这边选Codex 你这边就可以看到额度了 这个就跟我们刚才一开始的那个网站 给的额度是一样的 好了 接下来我们就来对接我们的Codexco 袋x 现在它是需要对接的对不对

### Checking Quotas and Preparing to Connect Local Codex

**12:45** · 那我们先关掉它 我们回到我们的windows windows找到我们的用户管理员 就是你常用的 找到我们的Codex 这边有个Codex 那基本上是要这两个文件啊 一个是它的配置文件 一个是你的Keyy存放文件 这两个文件你都把命名命好 我们把它先删掉 把这个地址拿过来用粘贴进来

### Finding Codex Config Files on Windows

**13:08** · 改成你自己的 当然这个呢就会比较麻烦一点 但是如果你搞好了 那就非常方便了 这是V1 V1的 这边的gpt 你可以调成5.5 关掉把这个改一下 往下拉把这个复制一下 这个就是我们刚才生成的 右键进来有Codex 可能你接第三方就会像这样的 麻烦一点 对我们确实没问题 把这两个文件想复制出来 直接粘到这边来 把它覆盖好 覆盖好以后我们再登录Codex

### Editing API URL, Model, and Key Configuration

### Replacing Config Files and Restarting Codex

**13:40** · Codex回车没有退出干净 要退出干净的 要不然他可能还是会存在一些问题 好了接下来呢 我们就进来了 我们现在接下来进来的就非官方到了 我们试一下 先把这个沙盒开启 你看它这个速度 要比你从官网那边要快很多 所以有条件的小伙伴 就像我们今天这期 对接中转站Codex 那两个配置文件的方式 我会放在博客上 小伙伴们到时候自取或者私我

### Verifying Codex Is Connected to the Self-Hosted API Gateway

**14:10** · 我到时候发大家也行 他为什么会到网页中搜索 他说已经不是官方的API 好了那就很简单了 他告诉我们 其实指向的是我们这样的一个地址 这是我们刚才创建的那个中转站 那现在用的其实就是我们中转站 那我们回到中转站看一下 我们这个配置文件 选择Codex 然后我们可以发现我们这边有小绿点 就表明我们当前这个是已经在使用的

### Checking API Usage Status in the Gateway Dashboard

**14:37** · 如果你走第三方 那这边就不会显示用量了 它可以告诉我们 已通过API方式来登录 所以无缝对接的 你也可以让他去使用其他的 你这个站点的其他模型 可能都在这个地方能看到 如果你接什么Antigravity 接什么Claude Code那边都可以去这样的去使用

**14:56** · 好了这就是今天要跟大家分享的 open ai旗下的codex的使用方法 用官方的 并且讲了 用第三方自己自建的 这种中转站的方式 自建中转战方式 可能会速度更稳定一点 官方由于网络原因可能会稍慢一点 但是日常使用应该没什么太大问题

### Summary: Official Login vs Self-Hosted API Gateway

**15:19** · 那欢迎大家的收看 我会把相关的内容 相关的配置都给贴上去 大家可以直接复制去直接使用 好吧那欢迎大家收看 那我们下期节目再见拜拜