# 常见问题

## 以太坊

??? faq "什么是以太坊？"

    有几种方法可以回答这个问题，在专门的维基页面[什么是以太坊？][1]上介绍.

    如果您喜欢通过观看视频进行学习，请参阅:

    + [介绍以太坊][2] (Video, 10mi)
    + [Vitalik Buterin在比特币迈阿密2014展示Ethereum][3] (Video, 28min)
    + [奇点1对1：以太坊是一个分散的共识平台][4] (Video, 69min)
    + [我们的第二个Reddit“问我任何问题”社区选择的问题][5] (not actually a video)

??? faq "我如何购买Ether或ETH？"

    阅读[这里][6].

??? faq "我在哪里可以了解更多有关以太坊的信息？"

    - [主站][7]
    - [论坛][8]
    - [Github][9]
    - [博客][10]
    - [维基][11] [已经不工作了]
    - [聚会][12]
    - [白皮书][13]
    - [黄皮书][14]
    - [Facebook][15]
    - [Youtube][16]
    - [Google+][17]
    - [IRC Freenode][18](#ethereum) for weblink)
    - [堆换][19]

??? faq "我在哪里可以找到主要的项目知识库？"

    + [go-ethereum][20] ([@obscuren][21], [@maran][22])
    + [Parity][23]
    + [cpp-ethereum][24] ([@gavofyork][25], [@programmerTim][26], [@caktux][27])
    + [pyethereum][28] ([@vbuterin][29], [@heikoheiko][30], [@chenhouwu][31])
    + [ethereumj][32] ([@romanman][33], [@nicksavers][34])
    + [ethereumjs-lib][35] ([@ethers][36], [@wanderer][37])
    + Github上的[以太坊组织][9]

??? faq "我在哪里可以了解以太币的销售和挖矿？"

    + [比特币售卖常见问题][38]
    + [挖矿常见问题][39]

## 钱包

如果你的钱包有问题, 交易或其他有关使用以太网加密货币或进行交易的事宜, 看到这个[MyEtherWallet知识库][40]. 它回答了许多常见问题，如:

* [交易没有显示出来，或者永远在等待着][41]
* [ETH或代币发送到或从交易没有显示/交易说完成，但资金尚未显示][42]
* [由于Slack/Reddit/Google广告上的网络钓鱼信息，网络钓鱼，黑客，盗窃和被盗资金][43]

## 客户端

??? faq "哪里可以找到官方版本?"

    客户端:

    + [Geth 版本][44] (Go)
    + [Parity 版本][45] (Rust)
    + [eth 版本][46] (C++)
    + [Pyethereum 版本][47] (Python)
    + [Ethereumj 版本][48] (Java)
    + [EthereumJS 版本][49] (Javascript)

    其它:

    + [Mist 版本][44] (wallet and browser for dapps)
    + [Pyethapp 版本][50] (Python)
    + [Py-EVM 版本][51] (Python)

??? faq "如何安装开发版本?"

    + Homebrew
        + [Homebrew以太坊][52] ([@caktux][27])
    + 指南
        + [eth/AlethZero OSX超级简易安装指南][53] ([@stephantual][54])
        + [Go-Ethereum 简单的OSX编译指南][55] ([@stephantual][54])
        + [建立在Ubuntu上][56]
    + 构建
      + [Ethdev Buildbot][57]

??? faq "如何从源代码安装客户端？"

    + [构建 eth/AlethZero (C++)][58]
    + [构建 Mist (Go)][59]
    + [安装 Pyethereum (Python)][60]
    + [安装 EthereumJ (Java)][61]
    + [安装 Ethereumjs-lib (用于浏览器和节点的JavaScript)][62]

## 采矿

??? faq "我怎样才能挖掘以太币?"

    使用 eth/AlethZero

    + 处理交易
        + Disable "Debug" > "Force Mining"
        + Click "Mine"
    + 强制挖矿（谨慎使用，除非压力测试）
        + Enable "Debug" > "Force Mining"
        + Click "Mine"

    使用 eth 客户端

    ```
    # 只有强制挖矿到以太或压力测试
    $ eth --force-mining --mining on [YOUR OPTIONS...]
    ```

## 合约

??? faq "我在哪里可以了解合约开发？"

    + 文章
        + [以太坊开发教程][63]
    + 视频
        + [Ethereum][64]
        + [EtherCasts][65]

??? faq "我在哪里可以学类Python语言Serpent？"

    + 说明
        + [蛇的语言][66]
    + 示例
        + [维塔利克的蛇的例子][67]
    + 手册
        + [Pyethereum和蛇编程指南][68]
    + 视频
        + [用Vitalik学习以太坊][69]

??? faq "我哪里可以学类Lisp语言 LLL?"

    + 说明书
        + [LLL语言][70]
    + 示例
        + [PoC 5的LLL示例][71]
    + 视频
        + [用Asm编程社会][72]

??? faq "我哪里可以学类JavaScript语言Solidity?"

    + 文档
        + [Solidity文档][73]

    + 手册
        + [合约书写的凝聚力][74]

??? faq "如何测试合约?"

    + [EVM合约模拟器][75] ([@EtherCasts][76])
    + [Pyethereum测试仪][77] ([@ethereum][9])

??? faq "在哪里可以找到示例合约？"

    + 蛇
        + [作者Vitalik Buterin][67] ([@vbuterin][29])
        + [作者EtherCasts][76] ([@EtherCasts][76])
        + [作者罗伯迈尔斯][78] ([@robmyers][79])
        + [作者泰勒Florez][80] ([@qualiabyte][81])
    + LLL
        + [作者加文伍德][71] ([@gavofyork][25])
        + [作者丹尼斯麦金农][82] ([@dennismckinnon][83])
        + [作者Doug A.][84] ([@dlle9][85])

## ÐApps

??? faq "我哪里可以学习以太坊接口?"

    + [用于C ++的PoC 6 API][86]
    + [Go的PoC 5 API][87]
    + [用于QML的PoC 6 API][88]
    + [用于JavaScript的PoC 7 API][89]

??? faq "我哪里可以学 ÐApp 开发?"

    + [发布自己的货币][90] ([@maran][22])

??? faq "我哪里可以找到 ÐApp 开发工具?"

    **官方**

    + [eth/AlethZero GUI客户端（C ++）][91]
    + [Eth命令行客户端（C ++）][92]
    + [LLLC编译器（C ++）][93]
    + [以太坊命令行客户端（Go）][94]
    + [Mist浏览器（Go）][20]
    + [Pyeth命令行客户端（Python）][95]
    + [蛇编译器（Python）][66]

    **社区**

    + [Emacs的LLL模式][96] ([@robmyers][79])
    + [Emacs蛇模式][97] ([@robmyers][79])
    + [EVM-Sim][75] ([@EtherCasts][76])
    + [MintChalk][98] ([@mintchalk][99])

??? faq "我哪里可以找到ÐApps示例?"

    + [dapp-bin][100] ([@ethereum][9])
    + [给币][101] ([@gavofyork][25])
    + [杰夫币][102] ([@obscuren][21])
    + [让它下雨][103] ([@EtherCasts][76])
    + [克罗诺斯][104] ([@mquandalle][105])
    + [艺术世界以太坊][78] ([@robmyers][79])
    + [加密硬币手表][106] ([@EtherCasts][76])
    + [奥卡姆运行][107] ([@d11e9][85])
    + [信托戴维斯][108] ([@EtherCasts][76])

## IRC

??? faq "我如何加入以太坊IRC频道?"

    + [在IRC上的以太坊开发社区聊天!][109]

## 更多问答

+ [@fivedogit写的常见问题][110]

[1]: http://github.com/ethereum/wiki/wiki/What-is-Ethereum
[2]: https://www.youtube.com/watch?v=Clw-qf1sUZg
[3]: http://youtu.be/l9dpjN3Mwps
[4]: http://youtu.be/fbEtivJIfIU
[5]: http://www.reddit.com/r/IAmA/comments/2bjmgb/hi_we_are_the_ethereum_project_team_ask_us/
[6]: https://github.com/ethereum/wiki/wiki/Ethereum-introduction.md#how-do-you-buy-and-sell-ether-the-currency-of-ethereum
[7]: https://www.ethereum.org
[8]: https://forum.ethereum.org
[9]: https://github.com/ethereum
[10]: https://blog.ethereum.org
[11]: http://wiki.ethereum.org
[12]: http://ethereum.meetup.com
[13]: https://github.com/ethereum/wiki/wiki/White-Paper
[14]: http://gavwood.com/paper.pdf
[15]: https://www.facebook.com/ethereumproject
[16]: http://www.youtube.com/ethereumproject
[17]: http://google.com/+EthereumOrgOfficial
[18]: http://bitly.com/IRC_ethereum
[19]: https://ethereum.stackexchange.com
[20]: https://github.com/ethereum/go-ethereum
[21]: https://github.com/obscuren
[22]: https://github.com/maran
[23]: https://github.com/paritytech/parity
[24]: https://github.com/ethereum/cpp-ethereum/
[25]: https://github.com/gavofyork
[26]: https://github.com/programmerTim
[27]: https://github.com/caktux
[28]: https://github.com/ethereum/pyethereum
[29]: https://github.com/vbuterin
[30]: https://github.com/heikoheiko
[31]: https://github.com/chenhouwu
[32]: https://github.com/ethereum/ethereumj
[33]: https://github.com/romanman
[34]: https://github.com/nicksavers
[35]: https://github.com/ethereum/ethereumjs-lib
[36]: https://github.com/ethers
[37]: https://github.com/wanderer
[38]: https://forum.ethereum.org/discussion/196/the-ether-sale-faq/p1
[39]: https://forum.ethereum.org/discussion/197/mining-faq-live-updates/p1
[40]: https://myetherwallet.github.io/knowledge-base/
[41]: https://myetherwallet.github.io/knowledge-base/transactions/transactions-not-showing-or-pending.html
[42]: https://myetherwallet.github.io/knowledge-base/faq/eth-or-tokens-not-showing-on-exchange.html
[43]: https://myetherwallet.github.io/knowledge-base/security/phish-hacks-thefts-and-stolen-funds-due-to-phishing.html
[44]: https://github.com/ethereum/go-ethereum/releases
[45]: https://github.com/paritytech/parity/releases
[46]: https://github.com/ethereum/cpp-ethereum/releases
[47]: https://github.com/ethereum/pyethereum/releases
[48]: https://github.com/ethereum/ethereumj/releases
[49]: https://github.com/ethereumjs
[50]: https://github.com/ethereum/pyethapp/releases
[51]: https://github.com/ethereum/py-evm/releases
[52]: https://github.com/caktux/homebrew-ethereum
[53]: https://forum.ethereum.org/discussion/1388/alethzero-super-easy-install-guide-for-osx
[54]: https://github.com/stephantual
[55]: http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx-now-with-one-line-install
[56]: https://github.com/ethereum/cpp-ethereum/wiki/Building-on-Ubuntu#user-content-trusty-1404
[57]: http://build.ethdev.com/waterfall
[58]: https://github.com/ethereum/cpp-ethereum#building-from-source
[59]: https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum%28Go%29
[60]: https://github.com/ethereum/pyethereum#quickstart
[61]: https://github.com/ethereum/ethereumj#maven
[62]: https://github.com/ethereum/ethereumjs-lib#install
[63]: https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial
[64]: https://www.youtube.com/user/ethereumproject/videos
[65]: https://www.youtube.com/user/EtherCasts/videos
[66]: https://github.com/ethereum/wiki/wiki/Serpent
[67]: https://github.com/ethereum/serpent/tree/master/examples
[68]: https://blog.ethereum.org/2014/04/10/pyethereum-and-serpent-programming-guide/
[69]: https://www.youtube.com/watch?v=nXYDfLCLmMs
[70]: https://github.com/ethereum/cpp-ethereum/wiki/LLL-PoC-6/7a575cf91c4572734a83f95e970e9e7ed64849ce
[71]: https://github.com/ethereum/cpp-ethereum/wiki/LLL-Examples-for-PoC-5/7a575cf91c4572734a83f95e970e9e7ed64849ce
[72]: https://www.youtube.com/watch?v=xO1AxsYAkU8
[73]: http://solidity.readthedocs.io/en/latest/index.html
[74]: https://dappsforbeginners.wordpress.com
[75]: https://github.com/EtherCasts/evm-sim/
[76]: https://github.com/EtherCasts
[77]: https://github.com/ethereum/pyethereum/blob/master/ethereum/tests/test_contracts.py
[78]: https://github.com/robmyers/artworld-ethereum
[79]: https://github.com/robmyers
[80]: https://github.com/qualiabyte/ethereum-contracts
[81]: https://github.com/qualiabyte
[82]: https://github.com/dennismckinnon/Ethereum-Contracts
[83]: https://github.com/dennismckinnon
[84]: https://github.com/d11e9/g3
[85]: https://github.com/d11e9
[86]: https://github.com/ethereum/cpp-ethereum/wiki/Client-Development-with-PoC-6
[87]: https://github.com/ethereum/go-ethereum/wiki/PoC-5-Public-Go-API
[88]: https://github.com/ethereum/go-ethereum/wiki/QML-PoC6-API
[89]: https://github.com/ethereum/wiki/wiki/JavaScript-API
[90]: http://hidskes.com/blog/2014/05/21/ethereum-dapp-development-for-web-developers/
[91]: https://github.com/ethereum/cpp-ethereum/wiki/Using-AlethZero
[92]: https://github.com/ethereum/cpp-ethereum/wiki/Using-Ethereum-CLI-Client
[93]: https://github.com/ethereum/cpp-ethereum/blob/develop/lllc/main.cpp
[94]: https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options
[95]: https://github.com/ethereum/pyethereum#interacting-with-the-network
[96]: https://github.com/robmyers/lll-mode
[97]: https://github.com/robmyers/serpent-mode
[98]: http://www.mintchalk.com/
[99]: https://github.com/mintchalk
[100]: https://github.com/ethereum/dapp-bin
[101]: http://gavwood.com/gavcoin.html
[102]: https://github.com/obscuren/jeffcoin
[103]: https://github.com/EtherCasts/make-it-rain
[104]: https://github.com/mquandalle/chronos
[105]: https://github.com/mquandalle
[106]: https://github.com/EtherCasts/cryptocoinwatch
[107]: https://github.com/d11e9/Occams-Run
[108]: https://github.com/EtherCasts/trustdavis
[109]: https://forum.ethereum.org/discussion/1495/chat-with-the-ethereum-dev-community-on-irc
[110]: https://docs.google.com/document/d/14EIe984_86Y-uuNm-a4EsVeD3eI4qAAlz_MZof1qkqM/