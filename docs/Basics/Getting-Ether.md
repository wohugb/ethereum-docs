# 获取Ether

!!! note ""

    请注意，由于Ether的波动性，您只能以风险资本购买Ether(或除DAI或USDT等稳定价格外的任何加密货币), 即您可以承受损失的金钱, .


为了获得以太，你需要：

* 使用集中或无信任的服务为Ether交易其他货币
* 不推荐: 因为它不是非常用户友好, 使用用户友好型[Mist][1]以太坊GUI钱包，从Beta 6版开始可以使用<http://shapeshift.io/>接口.
* 不建议: 成为以太坊矿工 ([看这里][2]. 请注意，这个[Homestead指南][3]已过时, 这与这个[挖矿wiki][4]相同). 然而，这并不是建议，因为以太坊将转变为股权证明，从而使采矿过时。 成为矿工将涉及投资采矿设备(几个GPU，如果需要可能还有其他硬件，如兼容的计算机)，在PoS实施之前不太可能获得投资回报。

## 不可靠服务

!!! note ""

    请注意，以太坊平台的特殊之处在于，智能合约支持可靠的服务，避免了在货币兑换交易中对可信第三方的需求, 即中间货币兑换业务。 请注意，这些服务可能仍然需要某种程度的信任，例如,在服务提供商。

已经推出的项目包括：

* [LocalEthereum](https://localethereum.com/)允许以太换取法定货币, 像Localbitcoin. "智能合约允许用户安全地彼此交换乙醚, 并在发生争议时指定一个可信的第三方来调解交易. 目前, 可信中介始终是localethereum.com, 但合同将在未来进行调整，以切换到基于声誉的分布式仲裁池." 这句话的来源在[这里](https://blog.localethereum.com/how-our-escrow-smart-contract-works/).
* [EtherDelta](https://etherdelta.com/). 我（James Ray）发现，2017年1月1日该网站上的图表并不是很好，因此我发送了一条[推文](https://twitter.com/JamesCRay01/status/953101168669999104)给予反馈。 这对于交易代币很有用。 不幸的是，我试图用少量的存款，以便我可以买一个令牌，但没有发生任何事情。

尚未发布的项目（截至2017年12月14日）包括：

* [BTCrelay][8]
    * [更多信息][9] (关于ETH/BTC双向挂钩，无需修改比特币代码).
    * [BTCrelay 审计][10]

## 中心交易所一览¶

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

您也可以搜索"<insert your country> Ether exchange"。

还有[这个][16]。欲了解更多信息（列出更多交流），请参阅[此处](https://github.com/ethereum/wiki/wiki/Getting-Ether:-further-info)。

## 中心固定利率交换

| 交易所           | 货币                  |
| ---------------- | --------------------- |
| [Shapeshift][17] | BTC, LTC, DOGE, Other |
| [Bity][18]       | BTC, USD, EUR, CHF    |

## 交易和价格分析

* ETH在[coinmarketcap][19]上进行详尽的上市交易
* 汇总主要ETH市场的实时统计数据:
    * [Tradeblock][20]
    * [EthereumWisdom][21]
    * [Cryptocompare][22]
    * [Coinmarketcap][23]
* [EthGasStation][24]对于在制定交易清单前检查设定的天然气价格很有用, 以及在过去1500个街区交易次数中检查`Gas Guzzlers或ETH合同[前10名][25]

## 在线钱包，纸钱包和冷库

!!! todo

    这里仅仅是链接和笔记的倾销地 
    请将此列表转到生态系统

    在这里保留示例, 也许解释偏执的做法, 列出危险

* Mist以太坊钱包
    * [发布下载][1]
    * [Mist以太坊钱包开发者预览][26] - 基础博客文章
    * [如何轻松设置Mist以太坊钱包][27] - 汤米经济学教程
* Kryptokit Jaxx
    * [Jaxx主站][28]
    * [移动版本][29]
* Etherwall
    * [Etherwall网][30]
    * [Etherwall源][31]
* MyEtherWallet
    * [MyEtherWallet网][32]
    * [MyEtherWallet源][33]
    * [Chrome扩展程序][34]
* 冷钱包
    * [Icebox][35] by [ConsenSys][36] - 基于集成HD钱包库的lightwallet的冷库。
    * [Reddit 讨论 1][37]
    * [如何设置冷钱包][38]
* 硬钱包
    * [reddit讨论2][39]
    * [reddit讨论3][40]
* 脑钱包
    * [脑袋不安全，不要使用它们][41]
    * 使用脑钱包要非常小心。阅读最近的争议: [brainwallets][42] vs [为什么脑钱包是最好的][43]
* Misc
    * [Kraken钱包清扫工具][44] - 预售钱包导入
    * [来安全地存储以太的推荐方法][45]
    * [如何购买和储存以太][46]
    * [外行介绍蛮力，为什么不使用脑袋][47]
    * [Pyeth销售工具][48]
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