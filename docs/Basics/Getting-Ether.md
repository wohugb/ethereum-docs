# 获取以太币

!!! note ""

    请注意，您只能购买以太网（或任何加密货币，除DAI或USDT之类的稳定货币） 即您可以承受的金钱， 因为以太的波动本质。

为了获得以太币，你需要：

* 以集中或无信赖的方式交易其他货币的乙醚
* 使用用户友好[迷雾以太坊GUI钱包][1] Beta 6引入了使用http://shapeshift.io/ API购买ether的能力。
* 成为以太坊矿工 (看[这里][2]. 请注意[这][3] 宅基导游已经过时了， 这和[这个][4]是一样的 挖掘wiki).然而，这是不被推荐的，因为以太坊将转变为股权证明， 使采矿过时。 成为一名矿工将涉及到投资一个采矿设备（几个GPU，如果需要的话还可能有其他硬件，如兼容的计算机），这是不太可能在PoS实施之前获得投资回报。

## 不可靠服务

请注意，以太坊平台的特殊之处在于，智能合约可实现不受信任的第三方在货币兑换交易中的需求，,中间货币兑换业务。

已经启动的项目包括：

* [EtherDelta][5]. 我发现网站上的图表在2017年1月1日并不是很好，所以我发了一封[给予反馈的推特][6].这对于交易代币是很好的。 不幸的是，我试图用少量的存款，以便我可以买一个令牌，但什么都没有发生。
* [Decentrex][7]. 现在这个关闭了， 但是如果您通过点击高级设置来获取证书错误，您仍然可以访问该网站：“与其”母亲“etherdelta相比，由于体积非常低，用户关注度非常低，所以decentrex开放测试版项目已经关闭，没有进一步的支持（decentrex,是一个etherdelta分叉，就像你可以在github上找到的许多其他的etherdelta分叉），并且由于以太坊网络费用高和Ethereum网络不稳定的事实。

尚未发布的项目（截至2017年12月14日）包括：

* [BTCrelay][8]
    * [更知情][9] (关于ETH/BTC双向挂钩，无需修改比特币代码).
    * [BTCrelay 审计][10]
* [EtherEx 分散交换][11]. (此链接不再有效. [Etherex在Github上不活跃][12].)

此外，还有[LocalEthereum][13]，允许以太网兑换法定货币，如[Localbitcoin][14]。 “聪明的合同允许用户彼此安全地交换乙醚，并且在出现争议的情况下命名可信的第三方来调解交易。 目前，可信中介始终是localethereum.com，但合同将在未来适应切换到基于信誉的分布式仲裁池。" 这个引用的来源是[这里][15].

## 集中交易所一览¶

| 交易所       | 货币                    |
| ------------ | ----------------------- |
| Poloniex     | BTC                     |
| Kraken       | BTC, USD, EUR, CAD, GBP |
| Gatecoin     | BTC, EUR                |
| Bitfinex     | BTC, USD                |
| Bittrex      | BTC                     |
| Bluetrade    | BTC, LTC, DOGE          |
| HitBTC       | BTC                     |
| Livecoin     | BTC                     |
| Coinsquare   | BTC                     |
| Bittylicious | GBP                     |
| BTER         | CNY                     |
| Yunbi        | CNY                     |
| Metaexchange | BTC                     |

您也可以搜索“<您的国家>”以太交换“。

还有[这个][16]并查看文章底部的链接。

## 集中固定利率交换

| 交易所                             | 货币                  |
| ---------------------------------- | --------------------- |
| [Shapeshift][17] | BTC, LTC, DOGE, Other |
| [Bity][18]           | BTC, USD, EUR, CHF    |

## 交易和价格分析

* ETH markets exhaustive listing by volume on [coinmarketcap][19]
* Aggregating realtime stats of major ETH markets:
    * [Tradeblock][20]
    * [EthereumWisdom][21]
    * [Cryptocompare][22]
    * [Coinmarketcap][23]
* [EthGasStation][24] is useful for checking the gas price to set before making a transaction lists, as well as checking `Gas Guzzlers, or the [Top 10 ETH Contracts By Transaction Count Over the Last 1,500 Blocks][25]

## 在线钱包，纸钱包和冷库

!!! todo
    This is here just a dumping ground of links and notes
    Please move this over in a listing form to ecosystem

    Keep examples here, maybe explain paranoid practices, list dangers

* 雾醚包
    * [发布下载][1]
    * [雾醚包开发者预览][26] - foundation blog post
    * [如何轻松设置雾醚包][27] - Tutorial by Tommy Economics
* Kryptokit Jaxx
    * [Jaxx主站][28]
    * [移动版本][29]
* 醚墙
    * [醚墙网][30]
    * [醚墙源][31]
* 我醚包
    * [我醚包网][32]
    * [我醚包源][33]
    * [Chrome extension][34]
* 冷钱包
    * [Icebox][35] by [ConsenSys][36] - Cold storage based on lightwallet with HD wallet library integrated.
    * [Reddit 讨论 1][37]
    * [如何设置冷钱包][38]
* 硬钱包
    * [reddit讨论2][39]
    * [reddit讨论3][40]
* 脑钱包
    * [脑袋不安全，不要使用它们][41]
    * Extreme caution with brain wallets. Read the recent controversy: [brainwallets][42] vs [why-brain-wallet-is-the-best][43]
* Misc
    * [Kraken钱包清扫工具][44] - Pre-sale wallet import
    * [来安全地存储以太币的推荐方法][45]
    * [如何购买和储存乙醚][46]
    * [一个外行介绍暴力强迫，为什么不使用脑袋][47]
    * [Pyethsaletool][48]
    * [帐户与钱包][49]

欲了解更多信息(列出更多交易所), 查看 [这里][50].
[1]: https://github.com/ethereum/mist/releases
[2]: https://forum.ethereum.org/discussion/8886/quick-start-guide-to-mine-ethereum/p1
[3]: https://ethereum-homestead.readthedocs.io/en/latest/mining.html
[4]: https://github.com/ethereum/wiki/wiki/Mining
[5]: https://etherdelta.com
[6]: https://twitter.com/JamesCRay01/status/953101168669999104
[7]: https://decentrex.com/
[8]: http://btcrelay.org/
[9]: https://medium.com/@ConsenSys/taking-stock-bitcoin-and-ethereum-4382f0a2f17
[10]: http://martin.swende.se/blog/BTCRelay-Auditing.html
[11]: https://etherex.org
[12]: https://github.com/etherex/etherex
[13]: https://localethereum.com/
[14]: https://Localbitcoin.com/
[15]: https://blog.localethereum.com/how-our-escrow-smart-contract-works/
[16]: https://myetherwallet.github.io/knowledge-base/faq/buying-selling-exchanging-eth-tokens-fiat.html
[17]: http://shapeshift.io
[18]: https://bity.com
[19]: https://coinmarketcap.com/currencies/ethereum/#markets
[20]: https://tradeblock.com/ethereum
[21]: http://ethereumwisdom.com
[22]: https://www.cryptocompare.com/coins/eth/overview
[23]: https://coinmarketcap.com/currencies/ethereum/
[24]: https://ethgasstation.info
[25]: https://ethgasstation.info/gasguzzlers.php
[26]: https://blog.ethereum.org/2015/09/16/ethereum-wallet-developer-preview/
[27]: https://www.youtube.com/watch?v=Z6lE0Ctaeqs
[28]: http://jaxx.io/
[29]: http://favs.pw/first-ethereum-mobile-app-released/#.VsHn_PGPL5c
[30]: http://www.etherwall.com/
[31]: https://github.com/almindor/etherwall
[32]: https://www.myetherwallet.com/
[33]: https://github.com/kvhnuke/etherwallet/
[34]: http://sebfor.com/myetherwallet-chrome-extension-release/
[35]: https://github.com/ConsenSys/icebox
[36]: https://consensys.net/
[37]: https://www.reddit.com/r/ethereum/comments/45uvmy/offline_cold_storage_question/offline_cold_storage_question
[38]: https://www.reddit.com/r/ethereum/comments/48wfbv/eli5_how_to_setup_a_cold_storage_wallet_as/
[39]: https://www.reddit.com/r/ethereum/comments/45siaq/hardware_wallet/
[40]: https://www.reddit.com/r/ethereum/comments/4521o4/crowdfunding_ethereum_hardware_cold_storage_wallet/
[41]: https://www.reddit.com/r/ethereum/comments/45y8m7/brain_wallets_are_now_generally_shunned_by/
[42]: https://reddit.com/r/ethereum/comments/43fhb5/brainwallets
[43]: http://blog.ether.camp/post/138376049438/why-brain-wallet-is-the-best
[44]: https://www.kraken.com/ether
[45]: http://ethereum.stackexchange.com/questions/1239/what-is-the-recommended-way-to-safely-store-ether
[46]: http://sebfor.com/how-to-buy-and-store-ether/
[47]: http://www.fastcompany.com/3056651/researchers-find-a-crack-that-drains-supposedly-secure-bitcoin-wallets
[48]: https://github.com/ethereum/pyethsaletool/blob/master/README.md
[49]: https://www.reddit.com/r/ethereum/comments/47j3r5/eli5_accounts_vs_wallet_contracts_on_mist/
[50]: https://github.com/ethereum/wiki/wiki/Getting-Ether:-further-info