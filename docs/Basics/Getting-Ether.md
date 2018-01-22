# 获取以太币

!!! note ""

    note that you should only buy Ether (or any cryptocurrency except for stable ones like DAI or USDT) with risk capital, i.e. money that you can afford to lose, because of the volatile nature of Ether.

为了获得醚，你需要：

* trade other currencies for ether using centralised or trustless services
* use the user friendly [Mist Ethereum GUI Wallet](https://github.com/ethereum/mist/releases) that as of Beta 6 introduced the ability to purchase ether using the http://shapeshift.io/ API.
* become an Ethereum miner (see [here](https://forum.ethereum.org/discussion/8886/quick-start-guide-to-mine-ethereum/p1). Note that [this](https://ethereum-homestead.readthedocs.io/en/latest/mining.html) Homestead guide is outdated, which is the same as [this](https://github.com/ethereum/wiki/wiki/Mining) mining wiki). However, this is not recommended as Ethereum is going to transition to proof-of-stake, making mining obsolescent. Becoming a miner would involve investing in a mining rig (several GPUs, plus maybe other hardware if needed, like a compatible computer), which is unlikely to get a return on investment before PoS is implemented.

## 不可靠服务

Note that the Ethereum platform is special in that the smart contracts enable trustless services that obviate the need for trusted third parties in a currency exchange transaction, ie. disintermediate currency exchange businesses.

Projects that have launched includes:

* [EtherDelta](https://etherdelta.com). I found that the charts on the site weren't very good on January 1, 2017, so I sent a [tweet giving feedback](https://twitter.com/JamesCRay01/status/953101168669999104). This is good for trading tokens. Unfortunately I tried to make a deposit with a small amount, so that I could then buy a token but nothing happened.
* [Decentrex](https://decentrex.com/). This is now closed, but you can still access the site if you get a certificate error by clicking on advanced settings: "decentrex open source beta project was closed with no further support because of very low volume and very low users attention compared with its “mother” etherdelta (decentrex was a fork of etherdelta like many other etherdelta forks you can find on github) and due to high Ethereum network fees and the fact that Ethereum network is unstable."

Projects that have not been released yet (as of Dec 14 2017) includes:

* [BTCrelay](http://btcrelay.org/)
    * [More information](https://medium.com/@ConsenSys/taking-stock-bitcoin-and-ethereum-4382f0a2f17) (about ETH/BTC 2-way peg without modifying bitcoin code).
    * [BTCrelay audit](http://martin.swende.se/blog/BTCRelay-Auditing.html)
* [EtherEx decentralised exchange](https://etherex.org). (This link no longer works. [Etherex is not active on Github](https://github.com/etherex/etherex).)

Additionally there is also [LocalEthereum](https://localethereum.com/), which allows exchanging Ether for fiat currencies, like [Localbitcoin](https://Localbitcoin.com/). "The smart contract allows users to safely exchange ether with one another, and to name a trusted third-party to mediate a trade if a dispute arises. Currently, the trusted mediator is always localethereum.com, but the contract will be adapted in the future to switch over to a reputation-based distributed arbitrator pool." The source of this quote is [here](https://blog.localethereum.com/how-our-escrow-smart-contract-works/).

## 集中交易所一览¶


|交易所                   |货币|
|-|-|
|Poloniex                |BTC
|Kraken                  |BTC, USD, EUR, CAD, GBP
|Gatecoin                |BTC, EUR
|Bitfinex                |BTC, USD
|Bittrex                 |BTC
|Bluetrade               |BTC, LTC, DOGE
|HitBTC                  |BTC
|Livecoin                |BTC
|Coinsquare              |BTC
|Bittylicious            |GBP
|BTER                    |CNY
|Yunbi                   |CNY
|Metaexchange            |BTC

You can also search for "`<insert your country>` Ether exchange".

There's also this: https://myetherwallet.github.io/knowledge-base/faq/buying-selling-exchanging-eth-tokens-fiat.html and see the link at the bottom of the article.

## 集中固定利率交换

|交易所                              |货币
|-|-
|[Shapeshift](http://shapeshift.io) |BTC, LTC, DOGE, Other
|[Bity](https://bity.com)           |BTC, USD, EUR, CHF

## 交易和价格分析

* ETH markets exhaustive listing by volume on [coinmarketcap](https://coinmarketcap.com/currencies/ethereum/#markets)
* Aggregating realtime stats of major ETH markets:
    * [Tradeblock](https://tradeblock.com/ethereum)
    * [EthereumWisdom](http://ethereumwisdom.com)
    * [Cryptocompare](https://www.cryptocompare.com/coins/eth/overview)
    * [Coinmarketcap](https://coinmarketcap.com/currencies/ethereum/)
* [EthGasStation](https://ethgasstation.info) is useful for checking the gas price to set before making a transaction lists, as well as checking `Gas Guzzlers, or the [Top 10 ETH Contracts By Transaction Count Over the Last 1,500 Blocks](https://ethgasstation.info/gasguzzlers.php)

## 在线钱包，纸钱包和冷库

!!! todo
    This is here just a dumping ground of links and notes
    Please move this over in a listing form to ecosystem

    Keep examples here, maybe explain paranoid practices, list dangers

* 雾醚包
    * [发布下载](https://github.com/ethereum/mist/releases)
    * [雾醚包开发者预览](https://blog.ethereum.org/2015/09/16/ethereum-wallet-developer-preview/) - foundation blog post
    * [如何轻松设置雾醚包](https://www.youtube.com/watch?v=Z6lE0Ctaeqs) - Tutorial by Tommy Economics
* Kryptokit Jaxx
    * [Jaxx主站](http://jaxx.io/)
    * [移动版本](http://favs.pw/first-ethereum-mobile-app-released/#.VsHn_PGPL5c)
* 醚墙
    * [醚墙网](http://www.etherwall.com/)
    * [醚墙源](https://github.com/almindor/etherwall)
* 我醚包
    * [我醚包网](https://www.myetherwallet.com/)
    * [我醚包源](https://github.com/kvhnuke/etherwallet/)
    * [Chrome extension](http://sebfor.com/myetherwallet-chrome-extension-release/)
* 冷钱包
    * [Icebox](https://github.com/ConsenSys/icebox) by [ConsenSys](https://consensys.net/) - Cold storage based on lightwallet with HD wallet library integrated.
    * [Reddit 讨论 1](https://www.reddit.com/r/ethereum/comments/45uvmy/offline_cold_storage_question/offline_cold_storage_question)
    * [如何设置冷钱包](https://www.reddit.com/r/ethereum/comments/48wfbv/eli5_how_to_setup_a_cold_storage_wallet_as/)
* 硬钱包
    * [reddit讨论2](https://www.reddit.com/r/ethereum/comments/45siaq/hardware_wallet/)
    * [reddit讨论3](https://www.reddit.com/r/ethereum/comments/4521o4/crowdfunding_ethereum_hardware_cold_storage_wallet/)
* 脑钱包
    * [脑袋不安全，不要使用它们](https://www.reddit.com/r/ethereum/comments/45y8m7/brain_wallets_are_now_generally_shunned_by/)
    * Extreme caution with brain wallets. Read the recent controversy: [brainwallets](https://reddit.com/r/ethereum/comments/43fhb5/brainwallets) vs [why-brain-wallet-is-the-best](http://blog.ether.camp/post/138376049438/why-brain-wallet-is-the-best)
* Misc
    * [Kraken钱包清扫工具](https://www.kraken.com/ether) - Pre-sale wallet import
    * [来安全地存储以太币的推荐方法](http://ethereum.stackexchange.com/questions/1239/what-is-the-recommended-way-to-safely-store-ether)
    * [如何购买和储存乙醚](http://sebfor.com/how-to-buy-and-store-ether/)
    * [一个外行介绍暴力强迫，为什么不使用脑袋](http://www.fastcompany.com/3056651/researchers-find-a-crack-that-drains-supposedly-secure-bitcoin-wallets)
    * [Pyethsaletool](https://github.com/ethereum/pyethsaletool/blob/master/README.md)
    * [帐户与钱包](https://www.reddit.com/r/ethereum/comments/47j3r5/eli5_accounts_vs_wallet_contracts_on_mist/)

欲了解更多信息(列出更多交易所), 查看 [这里](https://github.com/ethereum/wiki/wiki/Getting-Ether:-further-info).