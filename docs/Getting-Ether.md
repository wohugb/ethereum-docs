Note that you should only buy Ether (or any cryptocurrency except for stable ones like DAI or USDT) with risk capital, i.e. money that you can afford to lose, because of the volatile nature of Ether.

In order to obtain Ether, you need to either:

* trade other currencies for ether using centralised or trustless services
* use the user friendly `Mist Ethereum GUI Wallet <https://github.com/ethereum/mist/releases>`_ that as of Beta 6 introduced the ability to purchase ether using the http://shapeshift.io/ API.
* become an Ethereum miner (see `here <https://forum.ethereum.org/discussion/8886/quick-start-guide-to-mine-ethereum/p1>`_. Note that `this <https://ethereum-homestead.readthedocs.io/en/latest/mining.html>`_ Homestead guide is outdated, which is the same as `this <https://github.com/ethereum/wiki/wiki/Mining>`_ mining wiki). However, this is not recommended as Ethereum is going to transition to proof-of-stake, making mining obsolescent. Becoming a miner would involve investing in a mining rig (several GPUs, plus maybe other hardware if needed, like a compatible computer), which is unlikely to get a return on investment before PoS is implemented.

Trustless services
--------------------------------------------------------------------------------

Note that the Ethereum platform is special in that the smart contracts enable trustless services that obviate the need for trusted third parties in a currency exchange transaction, ie. disintermediate currency exchange businesses.

Projects that have launched includes:

* `EtherDelta <https://etherdelta.com>`_. I found that the charts on the site weren't very good on January 1, 2017, so I sent a `tweet giving feedback <https://twitter.com/JamesCRay01/status/953101168669999104>`_. This is good for trading tokens. Unfortunately I tried to make a deposit with a small amount, so that I could then buy a token but nothing happened.
* `Decentrex <https://decentrex.com/>`_. This is now closed, but you can still access the site if you get a certificate error by clicking on advanced settings: "decentrex open source beta project was closed with no further support because of very low volume and very low users attention compared with its “mother” etherdelta (decentrex was a fork of etherdelta like many other etherdelta forks you can find on github) and due to high Ethereum network fees and the fact that Ethereum network is unstable."

Projects that have not been released yet (as of Dec 14 2017) includes:

* `BTCrelay <http://btcrelay.org/>`_
   * `More information <https://medium.com/@ConsenSys/taking-stock-bitcoin-and-ethereum-4382f0a2f17>`_ (about ETH/BTC 2-way peg without modifying bitcoin code).
   * `BTCrelay audit <http://martin.swende.se/blog/BTCRelay-Auditing.html>`_
* `EtherEx decentralised exchange <https://etherex.org>`_. (This link no longer works. `Etherex is not active on Github <https://github.com/etherex/etherex>`_.)

Additionally there is also `LocalEthereum <https://localethereum.com/>`_, which allows exchanging Ether for fiat currencies, like `Localbitcoin <https://Localbitcoin.com/>`_. "The smart contract allows users to safely exchange ether with one another, and to name a trusted third-party to mediate a trade if a dispute arises. Currently, the trusted mediator is always localethereum.com, but the contract will be adapted in the future to switch over to a reputation-based distributed arbitrator pool." The source of this quote is `here <https://blog.localethereum.com/how-our-escrow-smart-contract-works/>`_.

List of centralised exchange marketplaces
--------------------------------------------------------------------------------

========================== ============================
Exchange                   Currencies
========================== ============================
Poloniex                   BTC
Kraken                     BTC, USD, EUR, CAD, GBP
Gatecoin                   BTC, EUR
Bitfinex                   BTC, USD
Bittrex                    BTC
Bluetrade                  BTC, LTC, DOGE
HitBTC                     BTC
Livecoin                   BTC
Coinsquare                 BTC
Bittylicious               GBP
BTER                       CNY
Yunbi                      CNY
Metaexchange               BTC
========================== ============================

You can also search for "<insert your country> Ether exchange".

There's also this: https://myetherwallet.github.io/knowledge-base/faq/buying-selling-exchanging-eth-tokens-fiat.html and see the link at the bottom of the article.

Centralised fixed rate exchanges
-----------------------------------


========================== ============================
Exchange                   Currencies
========================== ============================
`Shapeshift`_              BTC, LTC, DOGE, Other
`Bity`_                    BTC, USD, EUR, CHF
========================== ============================

.. _Bity: https://bity.com
.. _Shapeshift: shapeshift.io


Trading and price analytics
--------------------------------------------------------------------------------

* `ETH markets exhaustive listing by volume on coinmarketcap <https://coinmarketcap.com/currencies/ethereum/#markets>`_
* Aggregating realtime stats of major ETH markets:

  * `Tradeblock <https://tradeblock.com/ethereum>`_
  * `EthereumWisdom <http://ethereumwisdom.com>`_
  * `Cryptocompare <https://www.cryptocompare.com/coins/eth/overview>`_
  * `Coinmarketcap <https://coinmarketcap.com/currencies/ethereum/>`_
* `EthGasStation <https://ethgasstation.info>`_ is useful for checking the gas price to set before making a transaction lists, as well as checking `Gas Guzzlers, or the 'Top 10 ETH Contracts By Transaction Count Over the Last 1,500 Blocks' <https://ethgasstation.info/gasguzzlers.php>`_

.. _online-wallets-and-storage-solutions:

Online wallets, paper wallets, and cold storage
================================================================================

.. todo::
  This is here just a dumping ground of links and notes
  Please move this over in a listing form to ecosystem

  Keep examples here, maybe explain paranoid practices, list dangers

* Mist Ethereum Wallet
    * `Releases to download <https://github.com/ethereum/mist/releases>`_
    * `Mist Ethereum Wallet developer preview <https://blog.ethereum.org/2015/09/16/ethereum-wallet-developer-preview/>`_ - foundation blog post
    * `How to easily set up the Ethereum Mist wallet! <https://www.youtube.com/watch?v=Z6lE0Ctaeqs>`_ - Tutorial by Tommy Economics
* Kryptokit Jaxx
    * `Jaxx main site <http://jaxx.io/>`_
    * `Mobile release <http://favs.pw/first-ethereum-mobile-app-released/#.VsHn_PGPL5c>`_
* Etherwall
    * `Etherwall website <http://www.etherwall.com/>`_
    * `Etherwall source <https://github.com/almindor/etherwall>`_
* MyEtherWallet
    * `MyEtherWallet website <https://www.myetherwallet.com/>`_
    * `MyEtherWallet source <https://github.com/kvhnuke/etherwallet/>`_
    * `Chrome extension <http://sebfor.com/myetherwallet-chrome-extension-release/>`_
* Cold storage
    * `Icebox <https://github.com/ConsenSys/icebox>`_ by `ConsenSys <https://consensys.net/>`_ - Cold storage based on lightwallet with HD wallet library integrated.
    * `Reddit discussion 1 <https://www.reddit.com/r/ethereum/comments/45uvmy/offline_cold_storage_question/offline_cold_storage_question>`_
    * `How to setup a cold storage wallet <https://www.reddit.com/r/ethereum/comments/48wfbv/eli5_how_to_setup_a_cold_storage_wallet_as/>`_
* Hardware wallet
    * `reddit discussion 2 <https://www.reddit.com/r/ethereum/comments/45siaq/hardware_wallet/>`_
    * `reddit discussion 3 <https://www.reddit.com/r/ethereum/comments/4521o4/crowdfunding_ethereum_hardware_cold_storage_wallet/>`_
* Brain wallet
    * brain wallets are not safe, do not use them. https://www.reddit.com/r/ethereum/comments/45y8m7/brain_wallets_are_now_generally_shunned_by/
    * Extreme caution with brain wallets. Read the recent controversy: https://reddit.com/r/ethereum/comments/43fhb5/brainwallets vs http://blog.ether.camp/post/138376049438/why-brain-wallet-is-the-best
* Misc
    * `Kraken Wallet Sweeper Tool <https://www.kraken.com/ether>`_ - Pre-sale wallet import
    * `Recommended ways to safely store ether <http://ethereum.stackexchange.com/questions/1239/what-is-the-recommended-way-to-safely-store-ether>`_
    * `How to buy and store ether <http://sebfor.com/how-to-buy-and-store-ether/>`_
    * `A laymen's intro into brute forcing and why not to use brain wallets <http://www.fastcompany.com/3056651/researchers-find-a-crack-that-drains-supposedly-secure-bitcoin-wallets>`_
    * `Pyethsaletool <https://github.com/ethereum/pyethsaletool/blob/master/README.md>`_
    * `Account vs wallet <https://www.reddit.com/r/ethereum/comments/47j3r5/eli5_accounts_vs_wallet_contracts_on_mist/>`_

For further info (which lists more exchanges), see `here <https://github.com/ethereum/wiki/wiki/Getting-Ether:-further-info>`_.