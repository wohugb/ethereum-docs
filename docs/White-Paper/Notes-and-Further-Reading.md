# 笔记和进一步阅读

## 笔记

1. 一个复杂的读者可能会注意到，实际上比特币地址是椭圆曲线公钥的散列，而不是公钥本身。然而，它实际上是完全合法的密码术语，指的是公钥本身作为公钥。这是因为比特币的密码学可以被认为是一种定制的数字签名算法，其中公钥由ECC公钥的哈希组成，签名由与ECC签名串联的ECC公钥组成，并且验证算法涉及检查ECC,签名中的pubkey与作为公钥提供的ECC pubkey散列签名，然后根据ECC pubkey验证ECC签名。
2. 技术上，前11个区块的中位数。
3. 在内部，2和“CHARLIE”都是数字，后者是以大端基数256表示。数字可以是至少0和最多2<sup>256</sup>-1.

## 进一步阅读

1. [内在规定][1]
2. [智能财产][3]
3. [智能合约][20]
4. [B-money][7]
5. [可重用的工作证明][21]
6. [拥有著作权安全属性标题][9]
7. [比特币白皮书][22]
8. [名币][10]
9. [Zooko's triangle][23]
10. [彩币白皮书][2]
11. [主币白皮书][24]
12. [无心自治公司，比特币杂志][6]
13. [简化付款确认][25]
14. [梅克尔树][26]
15. [帕特丽夏树][27]
16. [鬼][18]
17. [StorJ和自治代理, Jeff Garzik][28]
18. [Mike Hearn在图灵节上的智能财产][29]
19. [以太坊 RLP][30]
20. [以太坊梅克勒帕特里夏树][31]
21. [默克尔合集树上的彼得·托德][32]

[1]: http://bitcoinmagazine.com/8640/an-exploration-of-intrinsic-value-what-it-is-why-bitcoin-doesnt-have-it-and-why-bitcoin-does-have-it/
[2]: https://docs.google.com/a/buterin.com/document/d/1AnkP_cVZTCMLIzw4DvsW6M8Q2JC0lIzrTLuoWu2z1BE/edit
[3]: https://en.bitcoin.it/wiki/Smart_Property
[7]: http://www.weidai.com/bmoney.txt
[9]: http://szabo.best.vwh.net/securetitle.html
[11]: http://namecoin.org/
[12]: http://en.wikipedia.org/wiki/Domain_Name_System
[13]: http://online-storage-service-review.toptenreviews.com/
[14]: http://en.wikipedia.org/wiki/Delegative_democracy
[15]: http://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/
[16]: http://www.cl.cam.ac.uk/~fms27/papers/2008-StajanoCla-cyberdice.pdf
[17]: http://hanson.gmu.edu/futarchy.html
[18]: https://eprint.iacr.org/2013/881.pdf
[19]: https://web.archive.org/web/20140623061815/http://sourceforge.net/p/bitcoin/mailman/message/31709140/
[20]: https://en.bitcoin.it/wiki/Contracts
[21]: http://www.finney.org/~hal/rpow/
[22]: http://bitcoin.org/bitcoin.pdf
[23]: http://en.wikipedia.org/wiki/Zooko's_triangle
[24]: https://github.com/mastercoin-MSC/spec
[25]: https://en.bitcoin.it/wiki/Scalability#Simplifiedpaymentverification
[26]: http://en.wikipedia.org/wiki/Merkle_tree
[27]: http://en.wikipedia.org/wiki/Patricia_tree
[28]: http://garzikrants.blogspot.ca/2013/01/storj-and-bitcoin-autonomous-agents.html
[29]: http://www.youtube.com/watch?v=Pu4PAMFPo5Y
[30]: https://github.com/ethereum/wiki/wiki/%5BEnglish%5D-RLP
[31]: https://github.com/ethereum/wiki/wiki/%5BEnglish%5D-Patricia-Tree
[32]: http://sourceforge.net/p/bitcoin/mailman/message/31709140/