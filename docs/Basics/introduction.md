# 以太坊简介

![Ethereum Homestead gold ingots](https://sustergy.files.wordpress.com/2017/05/ethereum-homestead-background-17.jpg?w=1000)

!!! note ""
    请注意，由于以太坊的发展速度太快，随着核心开发和无心应用不断推出, 本文的某些部分可能已经过时, 你可以帮助我们保持最新版本！

## 关于

[以太坊][Ethereum]是一个[无心][decentralized]区块链平台，用于“构建无阻碍的应用”，而Ether是这个平台上使用的加密货币。
以太坊已经有几种形式的描述，比如(第一和第三种资源是更一般的介绍，第二种是技术性介绍，而第二种是最新的。另外一个介绍可以在[这里][old-docs]找到，但是它已经过时了。尽管有些信息已经过时，但直到2018年1月1日，以太坊仍然保持向后兼容性，所以信息仍然是相关的.):

[Ethereum]: https://www.ethereum.org/
[decentralized]:https://medium.com/@VitalikButerin/the-meaning-of-decentralization-a0c92b76a274
[old-docs]: https://bitsonblocks.net/2016/10/02/a-gentle-introduction-to-ethereum/

- 下一代智能合约和无心应用平台 — [以太坊白皮书][White-Paper]
- 一个安全分散的广义交易分类账目和一个广义的具有共享状态的事务性单例机器，也被描述为[加密法以太坊黄皮书][Yellow-Paper]
- 开源"可编程的块链"— [Ethdocs][Ethdocs]

[White-Paper]: https://github.com/ethereum/wiki/wiki/White-Paper
[Yellow-Paper]: https://github.com/Ethereum-community/yellowpaper/blob/master/Paper.pdf
[Ethdocs]: http://ethdocs.org/en/latest/introduction/what-is-ethereum.html

让我们简单地分析下这些术语的含义

**无心**技术采用[点对点计算机网络][p2p]下面有一张照片, 并不受中央机构如政府或服务器管理员（如谷歌或Facebook）的意愿的影响，这可以帮助公众更好地做出决策。
[p2p]: https://en.wikipedia.org/wiki/Peer-to-peer

**区块链**意味着通过添加和验证之前创建的块的交易块来构建和保护货币, 从而形成“连”. 随着时间的推移，添加到链中的块变得越来越难以破解，因为它们被区块链对等网络中的更多节点所验证。

**Web3.0**是区块链技术:

- *Web 1.0*由发布内容的网站和被动地阅读/查看它的用户组成。
- *Web 2.0*使用用户交互功能, 如论坛（带投票和评论）, 交互按钮 (例如Facebook的交互: 喜欢👍,爱❤️,笑😆,哇😲,悲伤😢,愤怒😠), 分享（重新发布）, 但是这些互动对主站没有直接的经济影响,用户也不会从网站分的产生的收益.
- *Web 3.0*开始被定义为远离集中计算能力的服务器(被称为[客户端-服务器网络模型][cs])向为客户端提供服务的对等网络和区块链, [从权力集中以及来自国家和公司的集权化到网络化的个人][DemocracyEarth].

[cs]: https://en.wikipedia.org/wiki/Client%E2%80%93server_model
[DemocracyEarth]: https://github.com/DemocracyEarth/paper/blob/master/README.mediawiki

![server-based-network](https://sustergy.files.wordpress.com/2017/05/200px-server-based-network-svg.png)
![p2p-network](https://sustergy.files.wordpress.com/2017/05/200px-p2p-network-svg.png)

**加密货币**是指一种数字货币，通过硬件计算能力(被称为采矿或工作证明)或其他能源密集度较低的方法（如证据保证）来解决加密代码的交易.(下面有更多的细节)

像[ZK SNARK][ZK SNARKs]这样的零知识证明也可以用来使加密货币交易更私密或秘密(这与安全不同), 从而否定了在[以太坊企业联盟][entethalliance]等许可的私有网络上运行应用程序的需求.
以太坊在[椭圆曲线alt_bn128][pull-213]上使用预编译的合约进行加法和标量乘法运算, [配对检查][pull-212], 允许[zk-SNARKs][byzantium], 也可以看[这里][zk-snarks-under-the-hood], 如在[拜占庭难分叉][byzantium]实施.
Zcoin也展示了Zerocoin协议 (计划整合以太坊).

[ZK SNARKs]: https://crypto.stackexchange.com/questions/19884/what-are-snarks
[entethalliance]: https://entethalliance.org/
[pull-213]: https://github.com/ethereum/EIPs/pull/213
[pull-212]: https://github.com/ethereum/EIPs/pull/212
[zk-snarks-under-the-hood]: https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6
[finalized-eips-standards-that-have-been-adopted]: https://github.com/ethereum/EIPs#finalized-eips-standards-that-have-been-adopted
[byzantium]: https://blog.ethereum.org/2017/10/12/byzantium-hf-announcement/

## 使用

以太坊的平台部分比单纯的加密货币更有用。
有了它，你可以创建任何分散的应用程序(被称为dapp，它通过对等网络而不是集中的客户端-服务器网络工作)，所以功能只受限于程序可能做什么和不做什么，结果是程序员开发什么，但理论上它可以用于任何经济或治理活动。

### 无心应用

点击[这里][2]，查看无心应用列表.

### 市场分析

截至2018年1月9日，以太坊的市值为[1185亿美元][mk1]，自[二零一五年七月三十日][mk2]起可能已开始流通，而[二零一五年八月八日][mk3]首笔交易使用以太坊。
将其与当前最大的，最大的加密货币比特币相比较，它的市值为[2530亿美元][mk1]，自[2009年1月份][mk4]开始流通。
从技术上讲，以太坊的增长速度要快得多，而对于长期投资来说更重要的是（我不鼓励这种只会引起波动的猜测），基本面比比特币要好得多。
虽然比特币确实有更多的市场和货币，例如更多的实体将接受它作为一种付款方式，这个wiki的创建者预计，时间会改变这一点 (事实上，在2017年5月左右，以太坊的市值最近已经超过了比特币的[一半][mk5]).
另外，以太坊的交易数量也超过了[2017年11月22日][mk6]的几个加密货币。
不过，请注意这个[反驳][mk7].

[mk1]: https://cryptolization.com/ethereum
[mk2]: https://github.com/jamesray1/homestead-guide/blob/32d2fa4ccfa3d45f8493a673a08247450d55fea0/source/introduction/the-homestead-release.rst#milestones-of-the-ethereum-development-roadmap
[mk3]: https://www.etherchain.org/account/0x5abfec25f74cd88437631a7731906932776356f9
[mk4]: http://www.newyorker.com/reporting/2011/10/10/111010fa_fact_davis
[mk5]: https://seekingalpha.com/article/4077679-ethereum-blasts-20-billion-market-cap-half-bitcoin
[mk6]: https://www.reddit.com/r/ethereum/comments/7est9k/ethereum_is_now_processing_more_transactions_a/
[mk7]: https://www.reddit.com/r/ethereum/comments/7est9k/ethereum_is_now_processing_more_transactions_a/dq7a31u/

## 问题

以太坊也存在一些问题，比如不能充分扩展，没有完全分散的能源消耗，以及采矿和量子计算攻击。
凭借其庞大的存储[数据库][i1]，挖掘和架构需要运行一个完整的节点来挖掘或验证交易，它不是足够分散。
更多（过时，但仍然适用）的信息，在[这里][i2]，以及在[这里][i3]。

[i1]: https://www.reddit.com/r/ethtrader/comments/7axn5g/ethereum_blockchain_sizewe_have_a_problem
[i2]: https://ethereum.stackexchange.com/questions/143/what-are-the-ethereum-disk-space-needs#826
[i3]: https://github.com/ethereum/go-ethereum#full-node-on-the-main-ethereum-network

### 可扩展性

以太坊将需要扩大规模(成为“[世界电脑][s1]”)以处理比Visa([每秒处理数以万计的事务][s2][链接中CTRL + F 24,000])，万事达卡和美国运通的每秒处理更多的交易，而截至2017年12月30日的当前版本Ethereum 1.0在2017年12月22日星期五[处理了1103523笔交易的记录][s3]，即每秒12.77比交易。

[s1]: https://www.youtube.com/watch?v=j23HnORQXvs
[s2]: https://usa.visa.com/run-your-business/small-business-tools/retail.html
[s3]: https://web.archive.org/web/20171230005127/https://etherscan.io/chart/tx

请注意，[Ripple][s4]声称，它可以每秒处理一千次交易，而它可以通过付款渠道处理更多交易。 
虽然支付渠道通过将支付与结算分开来实现无限的可扩展性, 他们这样做不会产生通常与延迟结算相关的风险.
进一步指出睿波币通过权力下放，通过分[布式验证器或分布式服务器网络](s5)实现这一目标，而它被描述为[联盟协议][s6]。

[s4]: https://ripple.com/dev-blog/ripple-consensus-ledger-can-sustain-1000-transactions-per-second/
[s5]: https://ripple.com/build/xrp-ledger-consensus-process/
[s6]: https://wiki.ripple.com/Federation_protocol

还有更多可扩展的区块链使用委托证明（DPOS）共识协议，如Bitshares和Steem。
Bitshares显然可以处理[100,000 TPS][s6]。

[s6]: https://bitshares.org/technology/industrial-performance-and-scalability

更一般地说，为了更快的支付或更高的交易吞吐量，您需要减少共识协议中的验证者数量，或者减少另一个。
这是一个[折衷的三角形][s7]。
你可能有一个异构[分片的区块链][s8]，不同的[分片][s9]在这些属性之间有不同程度的平衡。
以太坊正在研究分片，其中包括使用[无状态客户端][s10],而更多的是在[这里][s11]。

[s7]: https://twitter.com/VladZamfir/status/932319930363494400
[s8]: https://twitter.com/VladZamfir/status/932320997021171712
[s9]: https://github.com/ethereum/sharding/blob/develop/docs/doc.md
[s10]: https://github.com/ethereum/sharding/blob/develop/docs/doc.md#stateless-clients
[s11]: https://ethresear.ch/t/the-stateless-client-concept/172/14

如果通过某个区块链或分片瞬间提高可伸缩性, 同时保持等待时间不变，则需要减少分散, 这减少了危害整个网络所需的攻击点的数量, 即减少分散化会降低安全性.

### 合约

***工作量证明/权益证明/其它证明方法***

破解密码(特别是通过反复试验发现每个块的随机数非常大)的挖掘过程需要大量的计算能力。
尽管如此，我猜测在考虑现有金融系统的能源消耗时，计算能力应该更小(考虑在法定货币的生命周期中提取和处理资源以制造硬币和钞票，铸造和印刷，银行的能源消耗和相关的能源消耗层次)。
尽管如此，以太坊等一些加密货币的开发者正在过渡到或者已经在使用一种不同的维护和创建块的方式(就像以太坊一样)，
欲了解更多信息，您可以在[这里][w1]看到这个权益维基百科证明文章(尽管请注意关于这篇文章的标题警告，由于严重依赖与主题关系过于密切的来源，因此这篇文章可能不是可验证的或中立的).
棘手的部分是让证明方法比工作证明更好地工作，正如PoS Wiki的批评部分所[概述][w1]的那样。

[w1]: https://en.wikipedia.org/wiki/Proof-of-stake

### 量机攻击

如果量子计算成为更高性能的以太坊的密码签名方案, 椭圆曲线数字签名算法(ECDSA), 将是不安全的。 然而，这些解决方案将在[君士坦丁堡发布][7]即将实施， 和 [EIP 86: 具有“**自定义加密**：用户可以升级到ed25519签名，Lamport哈希梯形图签名或者他们想要的其他任何计划，他们不需要坚持使用ECDSA”的交易来源和签名的抽象。 ,Lamport签名可以用于量子抗性算法][8].
更多的信息就是,[这里][9], [这里][10], 和[这里][11].

***没有任何科技产品可以成为万灵药***

对于人性的不断改善，人生之间需要在生活上有所平衡，在物质上有利于我们，在精神上也要使我们受益,科技有可能使一些人变得更好，有些人变得更糟,所以需要考虑如何实现技术来最大化[效用][12].
 对此的一个考虑是[在这里][13].

### 缺陷

以太坊有很多昂贵的错误，例如[这里][bug1]和[这里][bug2]列出的错误。

[bug1]: https://github.com/ethereum/wiki/wiki/Major-issues-resulting-in-lost-or-stuck-funds
[bug2]: https://github.com/ethereum/wiki/wiki/Bugs

## 以太币交易

参考[这里][26].

## 开发

你有兴趣学习与以太坊开发智能合约，也许开发一个真正有用的dapp并成为百万富翁？

看看[以太坊网站][27]! 然后，您可以[阅读Solidity文档][28].

- [黄皮书][29](请确保您阅读EIPs，因为从12月8日起，它不会与8月8日的最后一次提交保持同步，而君士坦丁堡的[EIPs][30]则是在10月份实施的).相反，我建议 ;
- 先学习Python，例如,学习[Python的难题][31])(我学会了使用它，这是相当不错的)，[Codecademy][32]，[Pydocs][33], [Coursera][34]等。了解Python对于用作[pyethereum][35]客户端的pyethereum来说是非常有用的，它可以实现Serenity和Sharding，以及[vyper][36]这个实验性的，安全的智能合约编程语言;
- [LLL][37](也看[这里][38]和[这里][39]);
- [JULIA][40], 一个用于不同以太坊虚拟机的中间语言;
- 客户端，如[Geth][41]，[Parity Tech][42] [Parity][43]下的一个单独的组织，以太坊基金会，[C++以太坊][44]，[Pyethereum][35];
- [Serenity][45];
- [sharding][46];
- [research][47] 如无状态客户端，分片，可扩展性改进，Casper等;
- [EWasM][48];
- 如果您对测试感兴趣，请参阅文档[此处][49], 以及[Github测试回购][50], [这里有一个精华（已经过时了）][51],和[网格在这里][52];和
- [许多其他存储库][53].

## 结束语

以太当然似乎是一个很好的投资, 也是使用法定货币的好选择， 同时也是一个不经济的业务的推动者, 由于交易成本较低.
这比中央银行在地方和全球范围内的贸易优势更具无心性。 有了以太坊上的治理应用和系统, 甚至有可能消除民族国家所跨越的阻碍边界。 通过消除这些边界，社会可以更加开放， 包容和公平。

但是，一切技术只能在一定程度上帮助人类和世界。 更重要的是每一个人都变得越来越幸福。 每个人都必须走进去，进入一种身心的平静， 这是幸福开始显现的时候， 并实行平衡的生活。 练习某些技巧，例如自我实现团契给予的技巧， 如每日克里亚瑜伽冥想，发展无条件的爱，开始在心里， 保持头脑之间的眉毛之间的点， 和道德生活， 帮助每个人体现出内心的幸福， 从那里， 随时向外表示幸福。

## 延伸阅读

- [另一个介绍][54]
- [MyEtherWallet知识库(适用于钱包问题)][55]
- [介绍(Frontier首发，过时)][56]
- [这是另一个介绍，在2017年11月][57]
- [以太坊在Gitter上][58]
- [以太坊研究论坛][59]
- [正确的施工卡斯帕原型][60]
- [正确的施工卡斯帕原型][61]
- [无国籍客户的概念][62]
- [以太坊2和替代PoS实施][63]
- [以太坊维基][64]
- [以太坊和爱他们的爱人][65]

[这篇文章][66]最初是在2017年5月在这里创建的，从那以后一直定期更新.
请随时向jamesray.eth的初始作者发送捐款， 或自己编辑它，或叉！

[2]: https://github.com/ethereum/wiki/wiki/Decentralized-apps-(dapps)
[7]: https://github.com/ethereum/wiki/wiki/Releases
[8]: https://github.com/ethereum/EIPs/pull/208
[9]: https://ethereum.stackexchange.com/a/13577/95840
[10]: https://blog.ethereum.org/2015/12/24/understanding-serenity-part-i-abstraction/
[11]: https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial
[12]: https://en.wikipedia.org/wiki/Utilitarianism
[13]: https://medium.com/@RhysLindmark/co-evolving-the-phase-shift-to-cryptocapitalism-by-founding-the-ethereum-commons-co-op-f4771e5f0c83
[14]: https://en.wikipedia.org/wiki/The_DAO_(organization
[15]: https://paritytech.io/the-multi-sig-hack-a-postmortem/
[16]: https://paritytech.io/security-alert/
[17]: https://paritytech.io/security-update/
[18]: https://paritytech.io/security-alert-2/
[19]: https://paritytech.io/parity-technologies-multi-sig-wallet-issue-update/
[20]: https://paritytech.io/a-postmortem-on-the-parity-multi-sig-library-self-destruct/
[21]: https://paritytech.io/on-classes-of-stuck-ether-and-potential-solutions/
[22]: https://github.com/ethereum/EIPs/issues/156
[23]: https://github.com/ethereum/EIPs/issues/156#issuecomment-2766829920
[24]: https://github.com/ethereum/EIPs/issues/156#issuecomment-307015852
[25]: https://github.com/ethereum/EIPs/blob/master/EIPS/eip-161.md#addendum-2017-08-15
[26]: https://github.com/ethereum/wiki/wiki/Getting-Ether
[27]: https://www.ethereum.org/
[28]: https://solidity.readthedocs.io/en/develop/
[29]: https://github.com/ethereum/yellowpaper/pull/376
[30]: https://github.com/ethereum/EIPs
[31](https://www.learnpythonthehardway.org/
[32]: https://www.codecademy.com/learn/learn-python
[33]: https://docs.python.org/3/
[34]: https://www.coursera.org/courses?languages=en&query=learn+python
[35]: https://github.com/ethereum/pyethereum
[36]: https://github.com/ethereum/Vyper
[37]: https://media.consensys.net/an-introduction-to-lll-for-ethereum-smart-contract-development-e26e38ea6c23
[38]: https://github.com/ethereum/solidity/tree/develop/liblll
[39]: https://github.com/ethereum/solidity/tree/develop/lllc
[40]: https://solidity.readthedocs.io/en/develop/julia.html
[41]: https://github.com/ethereum/go-ethereum
[42]: https://github.com/paritytech
[43]: https://github.com/paritytech/parity
[44]: https://github.com/ethereum/cpp-ethereum
[45]: https://github.com/ethereum/pyethereum/tree/serenity
[46]: https://github.com/ethereum/sharding/blob/develop/docs/doc.md
[47]: https://github.com/ethereum/research
[48]: https://github.com/ewasm
[49]: https://ethereum-tests.readthedocs.io/en/latest/
[50]: https://github.com/ethereum/tests
[51]: https://gist.github.com/Souptacular/fd197b1fac7c6d2660b0bef27a33ed40#lll-and-evm-stack-resources
[52]: https://gitter.im/ethereum/tests
[53]: https://github.com/ethereum
[54]: https://github.com/jamesray1/Ethereum-introduction/wiki/Ethereum-introduction
[55]: https://myetherwallet.github.io/knowledge-base/
[56]: https://ethereum.gitbooks.io/frontier-guide/content/ethereum.html
[57]: https://medium.com/@Ethereum_AI/ethereum-introduction-what-exactly-is-it-why-care-how-to-invest-9a627ab04408
[58]: https://gitter.im/ethereum
[59]: https://ethresear.ch/
[60]: https://ethresear.ch/t/the-correct-by-construction-casper-paper-prototype-published-at-devcon-tear-it-apart/196
[61]: https://ethresear.ch/t/latest-casper-basics-tear-it-apart/151/57
[62]: https://ethresear.ch/t/the-stateless-client-concept/172
[63]: https://ethresear.ch/t/ethereum-2-and-alternative-pos-implementations/190/7
[64]: https://en.wikipedia.org/wiki/Ethereum
[65]: https://www.reddit.com/r/ethtrader/comments/6jyn9y/ethereum_the_hodlors_that_love_them/
[66]: https://sustergy.wordpress.com/2017/05/18/why-buy-ether-and-how/