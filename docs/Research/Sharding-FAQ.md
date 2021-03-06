# 分片区块链

## 介绍

目前，在所有区块链协议中，每个节点都存储所有状态（账户余额，合同代码和存储等）并处理所有交易。
这提供了大量的安全性，但极大地限制了可伸缩性：区块链无法处理比单个节点更多的事务。
在很大程度上因为这个原因，比特币被限制在每秒3-7次交易，以太坊到7-15等。
然而，这提出了一个问题：是否有创建新机制的方法，只有小部分节点验证每个事务？,只要有足够多的节点验证每个事务，即系统仍然具有高度安全性，但又足够少以至于系统可以并行处理许多事务，那么我们是否可以不使用这种技术来大大增加区块链的吞吐量？

## 问题

??? faq "什么是解决问题的一些微不足道但有缺陷的方法？"

    “简单解决方案”有三大类，.
    首先是简单地放弃缩放个别区块链，而假设用户将使用许多不同的“altcoins”.
    这极大地增加了吞吐量，但是以安全为代价：使用这种方法的吞吐量的N因子增加必然伴随着安全性的N因子降低.
    因此，对于超过小的N值是可行的.

    因此，对于超过小的N值是可行的.
    这可以起作用，并且在某些情况下很可能是正确的处方，因为区块大小可能更多地受到政治限制，而不是现实技术考虑.
    但是不管个人对任何个别情况的看法如何，这种方法不可避免地会有其局限性：如果一个人走得太远，那么运行在消费硬件上的节点就会退出，网络将开始仅​​依靠少数运行区块链的超级计算机,，这可能导致很大的集中化风险.

    第三个是“合并采矿”，这是一种技术，其中有许多连锁店，但所有连锁店都拥有相同的采矿能力（或证明利益体系的利益）。,目前，Namecoin通过这样做从比特币区块链中获得大部分安全性.
    如果所有矿工都参与其中，理论上可以将吞吐量提高N倍，而不会影响安全性.
    但是，这也存在这样的问题，即它将每个矿工的计算和存储负载增加了N倍，因此实际上，这种解决方案仅仅是块大小增加的隐形形式.

    即使这被认为是可以接受的，但仍存在链条并非真正“捆绑在一起”的缺陷;,只需要少量的经济激励来说服矿工放弃或妥协一个特定的链条,这种可能性实际上是相当真实的，而且[实际的历史事件].
    这种可能性实际上是相当真实的，并且合并采矿链遭到攻击的[实际历史事件][1]以及明确提倡使用合并挖掘攻击作为[“治理”特征]的开发者[2][2] ,]，破坏对特定联盟而言不“有利”的连锁。

    如果只有少数矿工/矿池参与合并开采每个链，那么即将出现[集中化风险][3]，而合并采矿的安全效益也大大降低。

??? faq "这听起来像是有一种可扩展性的难题。,这个三难题是什么，我们可以突破它吗？"

    该三难局面声称，区块链系统最多只能拥有以下三种属性中的两种:

    - **Decentralization** (defined as the system being able to run in a scenario where each participant only has access to O(c) resources, ie.
    a regular laptop or small VPS)
    - **Scalability** (defined as being able to process O(n) \> O(c) transactions)
    - **Security** (defined as being secure against attackers with up to O(n) resources)

    In the rest of this document, we’ll continue using **c** to refer to the size of computational resources (including computation, bandwidth and storage) available to each node, and **n** to refer to the size of the ecosystem in some abstract sense; we assume that transaction load, state size, and the market cap of a cryptocurrency are all proportional to **n**.

??? faq "有人认为，由于梅特卡夫定律，加密货币的市值应该与n ^ 2成正比，而不是n。,他们有意见吗？"

    没有.

??? faq "为什么不？"

    Metcalfe’s law claims that the value of a network is proportional to the square of the number of users (n^2), because if a network has n users then the network has value for each user, but then the value for each individual user is itself proportional to the number of users because if a network has n users that’s n-1 potential connections through the network that each individual user could benefit from.

    In practice, [empirical research suggests][4] that the value of a network with n users is close to “n^2 proportionality for small values of n and (n × log n) proportionality for large values of n.” This makes sense because for small values, the argument holds true, but once a system gets bigger, two effects slow the growth down.
    First, growth in practice often happens in communities, and so in a medium-scale network the network often already provides most of the connections that each user cares about.
    Second, connections are often substitutes from each other, and you could argue that people only derive \~O(log(k)) value from having k connections – having 23 brands of deodorant to choose from is nice, but it’s not that much better than having 22 choices, whereas the difference between one choice and zero choices is very significant.

    Furthermore, even if the value of a cryptocurrency is proportional to O(k \* log(k)) with k users, if we accept the above explanation as the reason why this is the case, then that also implies that transaction volume is also O(k \* log(k)), as the log(k) value per user theoretically comes from that user exercising log(k) connections through the network, and state size should also in many cases grow with O(k \* log(k)) as there are at least some kinds of state that are relationship-specific rather than user-specific.
    Hence, assuming n = O(k \* log(k)) and basing everything off of **n** (size of the ecosystem) and **c** (a single node’s computing power) is a perfectly fine model for us to use.

??? faq "什么是一些中等简单但只能部分解决可伸缩性问题的方法？"

    Many sharding proposals (eg.
    [this early BFT sharding proposal from Loi Luu et al at NUS][5], as well as [this Merklix tree][6]<sup>[1][7]</sup> approach that has been suggested for Bitcoin) attempt to either only shard transaction processing or only shard state, without touching the other<sup>[2][8]</sup>.
    These efforts are admirable and may lead to some gains in efficiency, but they run into the fundamental problem that they only solve one of the two bottlenecks.
    We want to be able to process 10,000+ transactions per second without either forcing every node to be a supercomputer or forcing every node to store a terabyte of state data, and this requires a comprehensive solution where the workloads of state storage, transaction processing and even transaction downloading and re-broadcasting are all spread out across nodes.

    Particularly, note that this requires changes at the P2P level, as a broadcast model is not scalable since it requires every node to download and re-broadcast O(n) data (every transaction that is being sent), whereas our decentralization criterion assumes that every node only has access to O(c) resources of all kinds.

??? faq "那些不会试图“碎片化”任何东西的方法呢？"

    [Bitcoin-NG][9] can increase scalability somewhat by means of an alternative blockchain design that makes it much safer for the network if nodes are spending large portions of their CPU time verifying blocks.
    In simple PoW blockchains, there are high centralization risks and the safety of consensus is weakened if capacity is increased to the point where more than about 5% of nodes’ CPU time is spent verifying blocks; Bitcoin-NG’s design alleviates this problem.
    However, this can only increase the scalability of transaction capacity by a constant factor of perhaps 5-50x<sup>[3][10],[4][11]</sup>, and does not increase the scalability of state.
    That said, Bitcoin-NG-style approaches are not mutually exclusive with sharding, and the two can certainly be implemented at the same time.

    Channel-based strategies (lightning network, Raiden, etc) can scale transaction capacity by a constant factor but cannot scale state storage, and also come with their own unique sets of tradeoffs and limitations particularly involving denial-of-service attacks; on-chain scaling via sharding (plus other techniques) and off-chain scaling via channels are arguably both necessary and complementary.

    There exist approaches that use advanced cryptography, such as [Mimblewimble][12] and strategies based on ZK-SNARKs, to solve one specific part of the scaling problem: initial full node synchronization.
    Instead of verifying the entire history from genesis, nodes could verify a cryptographic proof that the current state legitimately follows from the history.
    These approaches do solve a legitimate problem, although it is worth noting that one can rely on cryptoeconomics instead of pure cryptography to solve the same problem in a much simpler way - see Ethereum’s current implementations of [fast syncing][13] and [warp syncing][14].
    Neither solution does anything to alleviate state size growth or the limits of online transaction processing.

??? faq "等离子如何适应三难困境？"

    In the event of a large attack on Plasma subchains, all users of the Plasma subchains would need to withdraw back to the root chain.
    If Plasma has O(N) users, then this will require O(N) transactions, and so O(N / C) time to process all of the withdrawals.
    If withdrawal delays are fixed to some D (ie.
    the naive implementation), then as soon as N > C * D, there will not be enough space in the blockchain to process all withdrawals in time, and so the system will be insecure; in this mode, Plasma should be viewed as increasing scalability only by a (possibly large) constant factor.
    If withdrawal delays are flexible, so they automatically extend if there are many withdrawals being made, then this means that as N increases further and further, the amount of time that an attacker can force everyone's funds to get locked up increases, and so the level of "security" of the system decreases further and further in a certain sense, as extended denial of access can be viewed as a security failure, albeit one milder than total loss of access.
    However, this is a different _direction_ of tradeoff from other solutions, and arguably a much milder tradeoff, hence why Plasma subchains are nevertheless a large improvement on the status quo.

??? faq "州的规模，历史，cryptoconomics，哦，我的！,在我们继续前进之前，定义一些这些术语！"

    -   **State**: a set of information that represents the “current state” of a system; determining whether or not a transaction is valid, as well as the effect of a transaction, should in the simplest model depend only on state.
    Examples of state data include the UTXO set in bitcoin, balances + nonces + code + storage in ethereum, and domain name registry entries in Namecoin.
    -    **History**: an ordered list of all transactions that have taken place since genesis.
    In a simple model, the present state should be a deterministic function of the genesis state and the history.
    -  **Transaction**: an object that goes into the history.
    In practice, a transaction represents an operation that some user wants to make, and is cryptographically signed.
    - **State transition function**: a function that takes a state, applies a transaction and outputs a new state.
    The computation involved may involve adding and subtracting balances from accounts specified by the transaction, verifying digital signatures and running contract code.
    -   **Merkle tree**: a cryptographic hash tree structure that can store a very large amount of data, where authenticating each individual piece of data only takes O(log(n)) space and time.
    See [here][15] for details.
    In Ethereum, the transaction set of each block, as well as the state, is kept in a Merkle tree, where the roots of the trees are committed to in a block.
    - **Receipt**: an object that represents an effect of a transaction that is not stored in the state, but which is still stored in a Merkle tree and committed to in a block so that its existence can later be efficiently proven even to a node that does not have all of the data.
    Logs in Ethereum are receipts; in sharded models, receipts are used to facilitate asynchronous cross-shard communication.
    - **Light client**: a way of interacting with a blockchain that only requires a very small amount (we’ll say O(1), though O(log(c)) may also be accurate in some cases) of computational resources, keeping track of only the block headers of the chain by default and acquiring any needed information about transactions, state or receipts by asking for and verifying Merkle proofs of the relevant data on an as-needed basis.
    - **State root**: the root hash of the Merkle tree representing the state<sup>[5][16]</sup>

    <center><img src="https://github.com/vbuterin/diagrams/raw/master/scalability_faq/image02.png" width="450"></img><br> <small><i>The Ethereum state tree, and how the state root fits into the block structure</i></small></center>

??? faq "分片背后的基本思想是什么？"

    We split the state up into K = O(n / c) partitions that we call “shards”.
    For example, a sharding scheme on Ethereum might put all addresses starting with 0x00 into one shard, all addresses starting with 0x01 into another shard, etc.
    In the simplest form of sharding, each shard also has its own transaction history, and the effect of transactions in some shard k are limited to the state of shard k.
    One simple example would be a multi-asset blockchain, where there are K shards and each shard stores the balances and processes the transactions associated with one particular asset.
    In more advanced forms of sharding, some form of cross-shard communication capability, where transactions on one shard can trigger events on other shards, is also included.

??? faq "分片背后的基本思想是什么？"

    A simple approach is as follows.
    There exist nodes called **collators** that accept transactions on shard `k` (depending on the protocol, collators either choose which `k` or are randomly assigned some `k`) and create **collations**.
    A collation has a **collation header**, a short message of the form "This is a collation of transactions on shard `k`.
    It expects the previous state root of shard `k` to be 0x12bc57, the Merkle root of the transactions in this collation is 0x3f98ea, and the state root after processing these transactions should be 0x5d0cc1.
    Signed, collators #1, 2, 4, 5, 6, 8, 11, 13 ...
    98, 99".

    A block must then contain a collation header for each shard.
    A block is valid if:

    1.
    The pre-state root given in each collation matches the current state root of the associated shard
    2.
    All transactions in all collations are valid
    3.
    The post-state root given in the collation matches the result of executing the transactions in the collation on top of the given pre-state
    4.
    The collation is signed by at least two thirds of the collators registered for that shard

    Note that there are now several "levels" of nodes that can exist in such a system:

    * **Super-full node** - processes all transactions in all collations and maintains the full state for all shards.
    * **Top-level node** - processes all top-level blocks, but does not process or try to download the transactions in each collation.
    Instead, it accepts it on faith that a collation is valid if two thirds of the collators in that shard say that it is valid.
    * **Single-shard node** - acts as a top-level node, but also processes all transactions and maintains the full state for some specific shard.
    * **Light node** - downloads and verifies the block headers of the top-level blocks only; does not process any collation headers or transactions unless it needs to read some specific entry in the state of some specific shard, in which case it downloads the Merkle branch to the most recent collation header for that shard and from there downloads the Merkle proof of the desired value in the state.

??? faq "这里面临的挑战是什么？"

    * **Cross shard communication** - the above design supports no cross-shard communication.
    How do we add cross-shard communication safely?
    * **Single-shard takeover attacks** - what if an attacker takes over the majority of the collators in one single shard, either to prevent any collations from getting enough signatures or, worse, to submit collations that are invalid?
    * **Fraud detection** - if an invalid collation does get made, how can nodes (including light nodes) be reliably informed of this so that they can verify the fraud and reject the collation if it is truly fraudulent?
    * **The data availability problem** - as a subset of fraud detection, what about the specific case where data is missing from a collation?
    * **Superquadratic sharding** - in the special case where n > c^2, in the simple design given above there would be more than O(c) collation headers, and so an ordinary node would not be able to process even just the top-level blocks.
    Hence, more than two levels of indirection between transactions and top-level block headers are required (ie.
    we need "shards of shards").
    What is the simplest and best way to do this?

    However, the effect of a transaction may depend on <i>events that earlier took place in other shards</i>; a canonical example is transfer of money, where money can be moved from shard i to shard j by first creating a “debit” transaction that destroys coins in shard i, and then creating a “credit” transaction that creates coins in shard j, pointing to a receipt created by the debit transaction as proof that the credit is legitimate.

??? faq "但CAP定理并不意味着完全安全的分布式系统是不可能的，因此分片是徒劳的？"

    The CAP theorem is a result that has to do with _distributed consensus_; a simple statement is: "in the cases that a network partition takes place, you have to choose either consistency or availability, you cannot have both".
    The intuitive argument is simple: if the network splits in half, and in one half I send a transaction "send my 10 coins to A" and in the other I send a transaction "send my 10 coins to B", then either the system is unavailable, as one or both transactions will not be processed, or it becomes inconsistent, as one half of the network will see the first transaction completed and the other half will see the second transaction completed.
    Note that the CAP theorem has nothing to do with scalability; it applies to any situation where multiple nodes need to agree on a value, regardless of the amount of data that they are agreeing on.
    All existing decentralized systems have found some compromise between availability and consistency; sharding does not make anything fundamentally harder in this respect.

??? faq "这里面临的挑战是什么？"

    The easiest scenario to satisfy is one where there are very many applications that individually do not have too many users, and which only very occasionally and loosely interact with each other; in this case, applications can live on separate shards and use cross-shard communication via receipts to talk to each other.

    This typically involves breaking up each transaction into a "debit" and a "credit".
    For example, suppose that we have a transaction where account A on shard M wishes to send 100 coins to account B on shard N.
    The steps would looks as follows:

    1.
    Send a transaction on shard M which (i) deducts the balance of A by 100 coins, and (ii) creates a receipt.
    A receipt is an object which is not saved in the state directly, but where the fact that the receipt was generated can be verified via a Merkle proof.
    2.
    Wait for the first transaction to be included (sometimes waiting for finalization is required; this depends on the system).
    3.
    Send a transaction on shard N which includes the Merkle proof of the receipt from (1).
    This transaction also checks in the state of shard N to make sure that this receipt is "unspent"; if it is, then it increases the balance of B by 100 coins, and saves in the state that the receipt is spent.
    4.
    Optionally, the transaction in (3) also saves a receipt, which can then be used to perform further actions on shard M that are contingent on the original operation succeeding.

    <img src="https://github.com/vbuterin/diagrams/raw/master/scalability_faq/image01.png" width="400"></img>

    In more complex forms of sharding, transactions may in some cases have effects that spread out across several shards and may also synchronously ask for data from the state of multiple shards.

??? faq "不同类型的应用程序如何适应分片区块链？"

    Some applications require no cross-shard interaction at all; multi-asset blockchains, and blockchains with completely heterogeneous applications that require no interoperability, are the simplest cases.
    If applications do need to talk to each other, the challenge is much easier if the interaction can be made asynchronous - that is, if the interaction can be done in the form of the application on shard A generating a receipt, a transaction on shard B “consuming” the receipt and performing some action based on it, and possibly sending a “callback” to shard A containing some response.
    Generalizing this pattern is easy, and is not difficult to incorporate into a high-level programming language.

    However, note that the in-protocol mechanisms that would be used for asynchronous cross-shard communication would be different and have weaker functionality compared to the mechanisms that are available for intra-shard communication.
    Some of the functionality that is currently available in non-scalable blockchains would, in a scalable blockchain, only be available for intra-shard communication.<sup>[7][17]</sup>.

??? faq "什么是火车旅馆问题？"

    The following example is courtesy of Andrew Miller.
    Suppose that a user wants to purchase a train ticket and reserve a hotel, and wants to make sure that the operation is atomic - either both reservations succeed or neither do.
    If the train ticket and hotel booking applications are on the same shard, this is easy: create a transaction that attempts to make both reservations, and throws an exception and reverts everything unless both reservations succeed.
    If the two are on different shards, however, this is not so easy; even without cryptoeconomic / decentralization concerns, this is essentially the problem of [atomic database transactions][18]).

    With asynchronous messages only, the simplest solution is to first reserve the train, then reserve the hotel, then once both reservations succeed confirm both; the reservation mechanism would prevent anyone else from reserving (or at least would ensure that enough spots are open to allow all reservations to be confirmed) for some period of time.
    However, this means that the mechanism relies on an extra security assumptions: that cross-shard messages from one shard can get included in another shard within some fixed period of time.

    With cross-shard synchronous transactions, the problem is easier, but the challenge of creating a sharding solution capable of making cross-shard atomic synchronous transactions is itself decidedly nontrivial.

    If an individual application has more than O(c) usage, then that application would need to exist across multiple chains.
    The feasibility of doing this depends on the specifics of the application itself; some applications (eg.
    currencies) are easily parallelizable, whereas others (eg.
    certain kinds of market designs) cannot be parallelized and must be processed serially.

    There are properties of sharded blockchains that we know for a fact are impossible to achieve.
    [Amdahl’s law][19] states that in any scenario where applications have any non-parallelizable component, once parallelization is easily available the non-parallelizable component quickly becomes the bottleneck.
    In a general computation platform like Ethereum, it is easy to come up with examples of non-parallelizable computation: a contract that keeps track of an internal value x and sets x = sha3(x, tx\_data) upon receiving a transaction is a simple example.
    No sharding scheme can give individual applications of this form more than O(c) performance.
    Hence, it is likely that over time sharded blockchain protocols will get better and better at being able to handle a more and more diverse set of application types and application interactions, but a sharded architecture will always necessarily fall behind a single-shard architecture in at least some ways at scales exceeding O(c).

??? faq "我们在什么样的安全模式下运行？"

    There are several competing models under which the safety of blockchain designs is evaluated:

    * **Honest majority** (or honest supermajority): we assume that there is some set of validators and up to ½ (or ⅓ or ¼) of those validators are controlled by an attacker, and the remaining validators honestly follow the protocol
    * **Uncoordinated majority**: we assume that all validators are rational in a game-theoretic sense (except the attacker, who is motivated to make the network fail in some way), but no more than some fraction (often between ¼ and ½) are capable of coordinating their actions.
    * **Coordinated choice**: we assume that all validators are controlled by the same actor, or are fully capable of coordinating on the economically optimal choice between themselves.
    We can talk about the **cost to the coalition** (or profit to the coalition) of achieving some undesirable outcome.
    * **Bribing attacker model**: we take the uncoordinated majority model, but instead of making the attacker be one of the participants, the attacker sits outside the protocol, and has the ability to bribe any participants to change their behavior.
    Attackers are modeled as having a **budget**, which is the maximum that they are willing to pay, and we can talk about their **cost**, the amount that they _end up paying_ to disrupt the protocol equilibrium.

    Bitcoin proof of work with [Eyal and Sirer’s selfish mining fix][20] is robust up to ½ under the honest majority assumption, and up to ¼ under the uncoordinated majority assumption.
    [Schellingcoin][21] is robust up to ½ under the honest majority and uncoordinated majority assumptions, has ε (ie.
    slightly more than zero) cost of attack in a coordinated choice model, and has a P + ε budget requirement and ε cost in a bribing attacker model due to [P + epsilon attacks][22].

    Hybrid models also exist; for example, even in the coordinated choice and bribing attacker models, it is common to make an **honest minority assumption** that some portion (perhaps 1-15%) of validators will act altruistically regardless of incentives.
    We can also talk about coalitions consisting of between 50-99% of validators either trying to disrupt the protocol or harm other validators; for example, in proof of work, a 51%-sized coalition can double its revenue by refusing to include blocks from all other miners.

    The honest majority model is arguably highly unrealistic and has already been empirically disproven - see Bitcoin's [SPV mining fork][23] for a practical example.
    It proves too much: for example, an honest majority model would imply that honest miners are willing to voluntarily burn their own money if doing so punishes attackers in some way.
    The uncoordinated majority assumption may be realistic; there is also an intermediate model where the majority of nodes is honest but has a budget, so they shut down if they start to lose too much money.

    The bribing attacker model has in some cases been criticized as being unrealistically adversarial, although its proponents argue that if a protocol is designed with the bribing attacker model in mind then it should be able to massively reduce the cost of consensus, as 51% attacks become an event that could be recovered from.
    We will evaluate sharding in the context of both uncoordinated majority and bribing attacker models.

??? faq "我们如何才能以不协调的多数模式解决单分片接管攻击？"

    In short, random sampling.
    Each shard is assigned a certain number of collators (eg.
    150), and the collators that approve blocks on each shard are taken from the sample for that shard.
    Samples can be reshuffled either semi-frequently (eg.
    once every 12 hours) or maximally frequently (ie.
    there is no real independent sampling process, collators are randomly selected for each shard from a global pool every block).

    The result is that even though only a few nodes are verifying and creating blocks on each shard at any given time, the level of security is in fact not much lower, in an honest/uncoordinated majority model, than what it would be if every single node was verifying and creating blocks.
    The reason is simple statistics: if you assume a ⅔ honest supermajority on the global set, and if the size of the sample is 150, then with 99.999% probability the honest majority condition will be satisfied on the sample.
    If you assume a ¾ honest supermajority on the global set, then that probability increases to 99.999999998% (see [here][24] for calculation details).

    Hence, at least in the honest / uncoordinated majority setting, we have:

    -   **Decentralization** (each node stores only O(c) data, as it’s a light client in O(c) shards and so stores O(1) \* O(c) = O(c) data worth of block headers, as well as O(c) data corresponding to the full state and recent history of one or several shards that it is assigned to at the present time)
    -  **Scalability** (with O(c) shards, each shard having O(c) capacity, the maximum capacity is n = O(c\^2))
    - **Security** (attackers need to control at least ⅓ of the entire O(n)-sized validator pool in order to stand a chance of taking over the network).

    In the Zamfir model (or alternatively, in the “very very adaptive adversary” model), things are not so easy, but we will get to this later.
    Note that because of the imperfections of sampling, the security threshold does decrease from ½ to ⅓, but this is still a surprisingly low loss of security for what may be a 100-1000x gain in scalability with no loss of decentralization.

??? faq "你如何在工作证明中进行这种抽样，以及证明利益？"

    In proof of stake, it is easy.
    There already is an “active validator set” that is kept track of in the state, and one can simply sample from this set directly.
    Either an in-protocol algorithm runs and chooses 150 validators for each shard, or each validator independently runs an algorithm that uses a common source of randomness to (provably) determine which shard they are at any given time.
    Note that it is very important that the sampling assignment is “compulsory”; validators do not have a choice of what shard they go into.
    If validators could choose, then attackers with small total stake could concentrate their stake onto one shard and attack it, thereby eliminating the system’s security.

    In proof of work, it is more difficult, as with “direct” proof of work schemes one cannot prevent miners from applying their work to a given shard.
    It may be possible to use [proof-of-file-access forms][25] of proof of work to lock individual miners to individual shards, but it is hard to ensure that miners cannot quickly download or generate data that can be used for other shards and thus circumvent such a mechanism.
    The best known approach is through a technique invented by Dominic Williams called “puzzle towers”, where miners first perform proof of work on a common chain, which then inducts them into a proof of stake-style validator pool, and the validator pool is then sampled just as in the proof-of-stake case.

    One possible intermediate route might look as follows.
    Miners can spend a large (O(c)-sized) amount of work to create a new “cryptographic identity”.
    The precise value of the proof of work solution then chooses which shard they have to make their next block on.
    They can then spend an O(1)-sized amount of work to create a block on that shard, and the value of that proof of work solution determines which shard they can work on next, and so on<sup>[8][26]</sup>.
    Note that all of these approaches make proof of work “stateful” in some way; the necessity of this is fundamental.

??? faq "抽样或多或少频繁的权衡是什么？"

    Selection frequency affects just how adaptive adversaries can be for the protocol to still be secure against them; for example, if you believe that an adaptive attack (eg.
    dishonest validators who discover that they are part of the same sample banding together and colluding) can happen in 6 hours but not less, then you would be okay with a sampling time of 4 hours but not 12 hours.
    This is an argument in favor of making sampling happen as quickly as possible.

    The main challenge with sampling taking place every block is that reshuffling carries a very high amount of overhead.
    Specifically, verifying a block on a shard requires knowing the state of that shard, and so every time validators are reshuffled, validators need to download the entire state for the new shard(s) that they are in.
    This requires both a strong state size control policy (ie.
    economically ensuring that the size of the state does not grow too large, whether by deleting old accounts, restricting the rate of creating new accounts or a combination of the two) and a fairly long reshuffling time to work well.

    Currently, the Parity client can download and verify a full Ethereum state snapshot via “warp-sync” in \~3 minutes; if we increase by 20x to compensate for increased usage (10 tx/sec instead of the current 0.5 tx/sec) (we’ll assume future state size control policies and “dust” accumulated from longer-term usage roughly cancel out) , we get \~60 minute state sync time, suggesting that sync periods of 12-24 hours but not less are safe.

    There are two possible paths to overcoming this challenge.

??? faq "我们是否可以强制更多的状态保持在用户端，以便事务可以被验证，而不需要验证器来保存所有状态数据？"

    The techniques here tend to involve requiring users to store state data and provide Merkle proofs along with every transaction that they send.
    A transaction would be sent along with a Merkle proof-of-correct-execution, and this proof would allow a node that only has the state root to calculate the new state root.
    This proof-of-correct-execution would consist of the subset of objects in the trie that would need to be traversed to access and verify the state information that the transaction must verify; because Merkle proofs are O(log(n)) sized, the proof for a transaction that accesses a constant number of objects would also be O(log(n)) sized.

    <img src="https://github.com/vbuterin/diagrams/raw/master/scalability_faq/image03.png" width="450"></img><br> <small><i>The subset of objects in a Merkle tree that would need to be provided in a Merkle proof of a transaction that accesses several state objects</i></small>

    Implementing this scheme in its pure form has two flaws.
    First, it introduces O(log(n)) overhead, although one could argue that this O(log(n)) overhead is not as bad as it seems because it ensures that the validator can always simply keep state data in memory and thus it never needs to deal with the overhead of accessing the hard drive<sup>[9][27]</sup>.
    Second, it can easily be applied if the addresses that are accessed by a transaction are static, but is more difficult to apply if the addresses in question are dynamic - that is, if the transaction execution has code of the form `read(f(read(x)))` where the address of some state read depends on the execution result of some other state read.
    In this case, the address that the transaction sender thinks the transaction will be reading at the time that they send the transaction may well differ from the address that is actually read when the transaction is included in a block, and so the Merkle proof may be insufficient<sup>[10][28]</sup>.

    A compromise approach is to allow transaction senders to send a proof that incorporates the most likely possibilities for what data would be accessed; if the proof is sufficient, then the transaction will be accepted, and if the state unexpectedly changes and the proof is insufficient then either the sender must resend or some helper node in the network resends the transaction adding the correct proof.
    Developers would then be free to make transactions that have dynamic behavior, but the more dynamic the behavior gets the less likely transactions would be to actually get included into blocks.

    Note that validators’ transaction inclusion strategies under this approach would need to be complicated, as they may spend millions of gas processing a transaction only to realize that the last step accesses some state entry that they do not have.
    One possible compromise is for validators to have a strategy that accepts only (i) transactions with very low gas costs, eg.
    \<100k, and (ii) transactions that statically specify a set of contracts that they are allowed to access, and contain proofs for the entire state of those contracts.
    Note that this only applies when transactions are initially broadcasted; once a transaction is included into a block, the order of execution is fixed, and so only the minimal Merkle proof corresponding to the state that actually needs to be accessed can be provided.

    If validators are not reshuffled immediately, there is one further opportunity to increase efficiency.
    We can expect validators to store data from proofs of transactions that have already been processed, so that that data does not need to be sent again; if k transactions are sent within one reshuffling period, then this decreases the average size of a Merkle proof from log(n) to log(n) - log(k).

??? faq "随机抽样的随机性如何产生？"

    First of all, it is important to note that even if random number generation is heavily exploitable, this is not a fatal flaw for the protocol; rather, it simply means that there is a medium to high centralization incentive.
    The reason is that because the randomness is picking fairly large samples, it is difficult to bias the randomness by more than a certain amount.

    The simplest way to show this is through the [binomial distribution][24], as described above; if one wishes to avoid a sample of size N being more than 50% corrupted by an attacker, and an attacker has p% of the global stake pool, the chance of the attacker being able to get such a majority during one round is:

    <img src="https://github.com/vbuterin/diagrams/raw/master/scalability_faq/image00.gif"></img>

    Here’s a table for what this probability would look like in practice for various values of N and p:

    <table> <tr><td>    </td><td> N = 50     </td><td> N = 100    </td><td> N = 150    </td><td> N = 250    </td> </tr><tr> <td> p = 0.4    </td><td> 0.0978     </td><td> 0.0271     </td><td> 0.0082     </td><td> 0.0009     </td> </tr><tr> <td> p = 0.33   </td><td> 0.0108     </td><td> 0.0004     </td><td> 1.83 * 10-5   </td><td> 3.98 * 10-8   </td> </tr><tr> <td> p = 0.25   </td><td> 0.0001     </td><td> 6.63 * 10<sup>-8</sup>   </td><td> 4.11 * 10<sup>-11</sup>  </td><td> 1.81 * 10-17  </td>< </tr><tr> <td> p = 0.2    </td><td> 2.09 * 10<sup>-6</sup>   </td><td> 2.14 * 10<sup>-11</sup>  </td><td> 2.50 * 10<sup>-16</sup>  </td><td> 3.96 * 10<sup>-26</sup>  </td> </tr></table>

    Hence, for N >= 150, the chance that any given random seed will lead to a sample favoring the attacker is very small indeed<sup>[11][29],[12][30]</sup>. What this means from the perspective of security of randomness is that the attacker needs to have a very large amount of freedom in choosing the random values order to break the sampling process outright.
    Most vulnerabilities in proof-of-stake randomness do not allow the attacker to simply choose a seed; at worst, they give the attacker many chances to select the most favorable seed out of many pseudorandomly generated options.
    If one is very worried about this, one can simply set N to a greater value, and add a moderately hard key-derivation function to the process of computing the randomness, so that it takes more than 2<sup>100</sup> computational steps to find a way to bias the randomness sufficiently.

    Now, let’s look at the risk of attacks being made that try to influence the randomness more marginally, for purposes of profit rather than outright takeover.
     For example, suppose that there is an algorithm which pseudorandomly selects 1000 validators out of some very large set (each validator getting a reward of $1), an attacker has 10% of the stake so the attacker’s average “honest” revenue 100, and at a cost of $1 the attacker can manipulate the randomness to “re-roll the dice” (and the attacker can do this an unlimited number of times).

    Due to the [central limit theorem][31], the standard deviation of the number of samples, and based [on other known results in math][32] the expected maximum of N random samples is slightly under M + S \* sqrt(2 \* log(N)) where M is the mean and S is the standard deviation.
    Hence the reward for manipulating the randomness and effectively re-rolling the dice (ie.
    increasing N) drops off sharply, eg.
    with 0 re-trials your expected reward is $100, with one re-trial it's $105.5, with two it's $108.5, with three it's $110.3, with four it's $111.6, with five it's $112.6 and with six it's $113.5.
    Hence, after five retrials it stops being worth it.
    As a result, an economically motivated attacker with ten percent of stake will (socially wastefully) spend $5 to get an additional revenue of $13, for a net surplus of $8.

    However, this kind of logic assumes that one single round of re-rolling the dice is expensive.
    Many older proof of stake algorithms have a “stake grinding” vulnerability where re-rolling the dice simply means making a computation locally on one’s computer; algorithms with this vulnerability are certainly unacceptable in a sharding context.
    Newer algorithms (see the “validator selection” section in the [proof of stake FAQ][33]) have the property that re-rolling the dice can only be done by voluntarily giving up one’s spot in the block creation process, which entails giving up rewards and fees.
    The best way to mitigate the impact of marginal economically motivated attacks on sample selection is to find ways to increase this cost.
    One method to increase the cost by a factor of sqrt(N) from N rounds of voting is the [majority-bit method devised by Iddo Bentov][34]; the Mauve Paper’s sharding algorithm expects to use this approach.

    Another form of random number generation that is not exploitable by minority coalitions is the deterministic threshold signature approach most researched and advocated by Dominic Williams.
    The strategy here is to use a [deterministic threshold signature][35] to generate the random seed from which samples are selected.
    Deterministic threshold signatures have the property that the value is guaranteed to be the same regardless of which of a given set of participants provides their data to the algorithm, provided that at least ⅔ of participants do participate honestly.
    This approach is more obviously not economically exploitable and fully resistant to all forms of stake-grinding, but it has several weaknesses:

    -   **It relies on more complex cryptography** (specifically, elliptic curves and pairings).
    Other approaches rely on nothing but the random-oracle assumption for common hash algorithms.
    -   **It fails when many validators are offline**.
    A desired goal for public blockchains is to be able to survive very large portions of the network simultaneously disappearing, as long as a majority of the remaining nodes is honest; deterministic threshold signature schemes at this point cannot provide this property.
    - **It’s not secure in a Zamfir model** where more than ⅔ of validators are colluding.
    The other approaches described in the proof of stake FAQ above still make it expensive to manipulate the randomness, as data from all validators is mixed into the seed and making any manipulation requires either universal collusion or excluding other validators outright.

    One might argue that the deterministic threshold signature approach works better in consistency-favoring contexts and other approaches work better in availability-favoring contexts.

    ??? faq "What are the concerns about sharding through random sampling in a bribing attacker or coordinated choice model?

    In a bribing attacker or coordinated choice model, the fact that validators are randomly sampled doesn’t matter: whatever the sample is, either the attacker can bribe the great majority of the sample to do as the attacker pleases, or the attacker controls a majority of the sample directly and can direct the sample to perform arbitrary actions at low cost (O(c) cost, to be precise).

    At that point, the attacker has the ability to conduct 51% attacks against that sample.
    The threat is further magnified because there is a risk of cross-shard contagion: if the attacker corrupts the state of a shard, the attacker can then start to send unlimited quantities of funds out to other shards and perform other cross-shard mischief.
    All in all, security in the bribing attacker or coordinated choice model is not much better than that of simply creating O(c) altcoins.

??? faq "我们该如何改进？"

    Basically, by comprehensively solving the problem of fraud detection.

    One major category of solution to this problem is the use of challenge-response mechanisms.
    Challenge-response mechanisms generally rely on a principle of escalation: fact X (eg.
    "collation #17293 in shard #54 is valid") is initially accepted as true if at least k validators sign a claim (backed by a deposit) that it is.
    However, if this happens, there is some challenge period during which 2k validators can sign a claim stating that it is false.
    If this happens, 4k validators can sign a claim stating that the claim is in fact true, and so forth until one side either gives up or most validators have signed claims, at which point every validator and client themselves checks whether or not X is true.
    If X is ruled true, everyone who made a claim saying so is rewarded and everyone who made a contradictory claim is penalized, and vice versa.

    Looking at this mechanism, you can prove that malicious actors lose an amount of money proportional to the number of actors that they forced to look at the given piece of data.
    Forcing *all* users to look at the data requires a large portion of validators to sign a claim which is false, which can be used to penalize all of them, so the cost of forcing all users to look at a piece of data is O(n); this prevents the challenge-response mechanism from being used as a denial-of-service vector.

??? faq "什么是数据可用性问题，我们如何使用纠删码来解决它？"

    See https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding

??? faq "我们可以通过某种奇特的密码累加器方案消除解决数据可用性的需求。"

    No.
    Suppose there is a scheme where there exists an object S representing the state (S could possibly be a hash) possibly as well as auxiliary information ("witnesses") held by individual users that can prove the presence of existing state objects (eg.
    S is a Merkle root, the witnesses are the branches, though other constructions like RSA accumulators do exist).
    There exists an updating protocol where some data is broadcasted, and this data changes S to change the contents of the state, and also possibly changes witnesses.

    Suppose some user has the witnesses for a set of N objects in the state, and M of the objects are updated.
    After receiving the update information, the user can check the new status of all N objects, and thereby see which M were updated.
    Hence, the update information itself encoded at least ~M * log(N) bits of information.
    Hence, the update information that everyone needs for receive to implement the effect of M transactions must necessarily be of size O(M).
    [14][36]

??? faq "那么这意味着我们实际上可以创建可扩展的分片区块链，其中发生任何不良事件的成本与整个验​​证器集的大小成正比？"

    There is one trivial attack by which an attacker can always burn O(c) capital to temporarily reduce the quality of a shard: spam it by sending transactions with high transaction fees, forcing legitimate users to outbid you to get in.
    This attack is unavoidable; you could compensate with flexible gas limits, and you could even try “transparent sharding” schemes that try to automatically re-allocate nodes to shards based on usage, but if some particular application is non-parallelizable, Amdahl’s law guarantees that there is nothing you can do.
    The attack that is opened up here (reminder: it only works in the Zamfir model, not honest/uncoordinated majority) is arguably not substantially worse than the transaction spam attack.
    Hence, we've reached the known limit for the security of a single shard, and there is no value in trying to go further.

??? faq "让我们回头一点。,如果我们有即时洗牌，我们是否真的需要这种复杂性？,不是即时洗牌基本上意味着每个分片直接从全局验证器池中提取验证器，因此它像块区链一样运行，因此分片实际上不会引入任何新的复杂性？"

    Kind of.
    First of all, it’s worth noting that proof of work and simple proof of stake, even without sharding, both have very low security in a bribing attacker model; a block is only truly “finalized” in the economic sense after O(n) time (as if only a few blocks have passed, then the economic cost of replacing the chain is simply the cost of starting a double-spend from before the block in question).
    Casper solves this problem by adding its finality mechanism, so that the economic security margin immediately increases to the maximum.
    In a sharded chain, if we want economic finality then we need to come up with a chain of reasoning for why a validator would be willing to make a very strong claim on a chain based solely on a random sample, when the validator itself is convinced that the bribing attacker and coordinated choice models may be true and so the random sample could potentially be corrupted.

??? faq "你提到透明分片。,我12岁了，这是什么？"

    Basically, we do not expose the concept of “shards” directly to developers, and do not permanently assign state objects to specific shards.
    Instead, the protocol has an ongoing built-in load-balancing process that shifts objects around between shards.
    If a shard gets too big or consumes too much gas it can be split in half; if two shards get too small and talk to each other very often they can be combined together; if all shards get too small one shard can be deleted and its contents moved to various other shards, etc.

    Imagine if Donald Trump realized that people travel between New York and London a lot, but there’s an ocean in the way, so he could just take out his scissors, cut out the ocean, glue the US east coast and Western Europe together and put the Atlantic beside the South Pole - it’s kind of like that.

??? faq "这有什么优点和缺点？"

    -   Developers no longer need to think about shards
    -   There’s the possibility for shards to adjust manually to changes in gas prices, rather than relying on market mechanics to increase gas prices in some shards more than others
    -   There is no longer a notion of reliable co-placement: if two contracts are put into the same shard so that they can interact with each other, shard changes may well end up separating them
    -   More protocol complexity

    The co-placement problem can be mitigated by introducing a notion of “sequential domains”, where contracts may specify that they exist in the same sequential domain, in which case synchronous communication between them will always be possible.
    In this model a shard can be viewed as a set of sequential domains that are validated together, and where sequential domains can be rebalanced between shards if the protocol determines that it is efficient to do so.

??? faq "同步交叉分片消息如何工作？"

    The process becomes much easier if you view the transaction history as being already settled, and are simply trying to calculate the state transition function.
    There are several approaches; one fairly simple approach can be described as follows:

    -   A transaction may specify a set of shards that it can operate in
    -   In order for the transaction to be effective, it must be included at the same block height in all of these shards.
    -   Transactions within a block must be put in order of their hash (this ensures a canonical order of execution)

    A client on shard X, if it sees a transaction with shards (X, Y), requests a Merkle proof from shard Y verifying (i) the presence of that transaction on shard Y, and (ii) what the pre-state on shard Y is for those bits of data that the transaction will need to access.
    It then executes the transaction and commits to the execution result.
    Note that this process may be highly inefficient if there are many transactions with many different “block pairings” in each block; for this reason, it may be optimal to simply require blocks to specify sister shards, and then calculation can be done more efficiently at a per-block level.
    This is the basis for how such a scheme could work; one could imagine more complex designs.
    However, when making a new design, it’s always important to make sure that low-cost denial of service attacks cannot arbitrarily slow state calculation down.

??? faq "那么半异步消息呢？"

    Vlad Zamfir created a scheme by which asynchronous messages could still solve the “book a train and hotel” problem.
    This works as follows.
    The state keeps track of all operations that have been recently made, as well as the graph of which operations were triggered by any given operation (including cross-shard operations).
    If an operation is reverted, then a receipt is created which can then be used to revert any effect of that operation on other shards; those reverts may then trigger their own reverts and so forth.
    The argument is that if one biases the system so that revert messages can propagate twice as fast as other kinds of messages, then a complex cross-shard transaction that finishes executing in K rounds can be fully reverted in another K rounds.

    The overhead that this scheme would introduce has arguably not been sufficiently studied; there may be worst-case scenarios that trigger quadratic execution vulnerabilities.
    It is clear that if transactions have effects that are more isolated from each other, the overhead of this mechanism is lower; perhaps isolated executions can be incentivized via favorable gas cost rules.
    All in all, this is one of the more promising research directions for advanced sharding.

??? faq "什么是保证跨分片调用？"

    One of the challenges in sharding is that when a call is made, there is by default no hard protocol-provided guarantee that any asynchronous operations created by that call will be made within any particular timeframe, or even made at all; rather, it is up to some party to send a transaction in the destination shard triggering the receipt.
    This is okay for many applications, but in some cases it may be problematic for several reasons:

    -   There may be no single party that is clearly incentivized to trigger a given receipt.
    If the sending of a transaction benefits many parties, then there could be **tragedy-of-the-commons effects** where the parties try to wait longer until someone else sends the transaction (ie.
    play "chicken"), or simply decide that sending the transaction is not worth the transaction fees for them individually.
    - **Gas prices across shards may be volatile**, and in some cases performing the first half of an operation compels the user to “follow through” on it, but the user may have to end up following through at a much higher gas price.
    This may be exacerbated by DoS attacks and related forms of **griefing**.
    -  Some applications rely on there being an upper bound on the “latency” of cross-shard messages (eg.
    the train-and-hotel example).
    Lacking hard guarantees, such applications would have to have **inefficiently large safety margins**.

    One could try to come up with a system where asynchronous messages made in some shard automatically trigger effects in their destination shard after some number of blocks.
    However, this requires every client on each shard to actively inspect all other shards in the process of calculating the state transition function, which is arguably a source of inefficiency.
    The best known compromise approach is this: when a receipt from shard A at height `height_a` is included in shard B at height `height_b`, if the difference in block heights exceeds `MAX_HEIGHT`, then all validators in shard B that created blocks from `height_a + MAX_HEIGHT + 1` to `height_b - 1` are penalized, and this penalty increases exponentially.
    A portion of these penalties is given to the validator that finally includes the block as a reward.
    This keeps the state transition function simple, while still strongly incentivizing the correct behavior.

??? faq "等等，但是如果攻击者同时从每个碎片向碎片X发送一个交叉碎片调用？,在数学上不可能及时包含所有这些呼叫吗？"

    Correct; this is a problem.
    Here is a proposed solution.
    In order to make a cross-shard call from shard A to shard B, the caller must pre-purchase “congealed shard B gas” (this is done via a transaction in shard B, and recorded in shard B).
    Congealed shard B gas has a fast demurrage rate: once ordered, it loses 1/k of its remaining potency every block.
    A transaction on shard A can then send the congealed shard B gas along with the receipt that it creates, and it can be used on shard B for free.
    Shard B blocks allocate extra gas space specifically for these kinds of transactions.
    Note that because of the demurrage rules, there can be at most GAS\_LIMIT \* k worth of congealed gas for a given shard available at any time, which can certainly be filled within k blocks (in fact, even faster due to demurrage, but we may need this slack space due to malicious validators).
    In case too many validators maliciously fail to include receipts, we can make the penalties fairer by exempting validators who fill up the “receipt space” of their blocks with as many receipts as possible, starting with the oldest ones.

    Under this pre-purchase mechanism, a user that wants to perform a cross-shard operation would first pre-purchase gas for all shards that the operation would go into, over-purchasing to take into account the demurrage.
    If the operation would create a receipt that triggers an operation that consumes 100000 gas in shard B, the user would pre-buy 100000 \* e (ie.
    271818) shard-B congealed gas.
    If that operation would in turn spend 100000 gas in shard C (ie.
    two levels of indirection), the user would need to pre-buy 100000 \* e\^2 (ie.
    738906) shard-C congealed gas.
    Notice how once the purchases are confirmed, and the user starts the main operation, the user can be confident that they will be insulated from changes in the gas price market, unless validators voluntarily lose large quantities of money from receipt non-inclusion penalties.

??? faq "凝结气体？,这听起来很有趣，不仅是跨分片操作，还有可靠的分片内调度"

    Indeed; you could buy congealed shard A gas inside of shard A, and send a guaranteed cross-shard call from shard A to itself.
    Though note that this scheme would only support scheduling at very short time intervals, and the scheduling would not be exact to the block; it would only be guaranteed to happen within some period of time.

??? faq "有保障的时间安排，包括碎片内和交叉碎片，是否有助于阻止大多数试图审查交易的共谋？"

    Yes.
    If a user fails to get a transaction in because colluding validators are filtering the transaction and not accepting any blocks that include it, then the user could send a series of messages which trigger a chain of guaranteed scheduled messages, the last of which reconstructs the transaction inside of the EVM and executes it.
    Preventing such circumvention techniques is practically impossible without shutting down the guaranteed scheduling feature outright and greatly restricting the entire protocol, and so malicious validators would not be able to do it easily.

??? faq "分片区块链可以更好地处理网络分区吗？"

    The schemes described in this document would offer no improvement over non-sharded blockchains; realistically, every shard would end up with some nodes on both sides of the partition.
    There have been calls (eg.
    from [IPFS’s Juan Benet][37]) for building scalable networks with the specific goal that networks can split up into shards as needed and thus continue operating as much as possible under network partition conditions, but there are nontrivial cryptoeconomic challenges in making this work well.

    One major challenge is that if we want to have location-based sharding so that geographic network partitions minimally hinder intra-shard cohesion (with the side effect of having very low intra-shard latencies and hence very fast intra-shard block times), then we need to have a way for validators to choose which shards they are participating in.
    This is dangerous, because it allows for much larger classes of attacks in the honest/uncoordinated majority model, and hence cheaper attacks with higher griefing factors in the Zamfir model.
    Sharding for geographic partition safety and sharding via random sampling for efficiency are two fundamentally different things.

    Second, more thinking would need to go into how applications are organized.
    A likely model in a sharded blockchain as described above is for each “app” to be on some shard (at least for small-scale apps); however, if we want the apps themselves to be partition-resistant, then it means that all apps would need to be cross-shard to some extent.

    One possible route to solving this is to create a platform that offers both kinds of shards - some shards would be higher-security “global” shards that are randomly sampled, and other shards would be lower-security “local” shards that could have properties such as ultra-fast block times and cheaper transaction fees.
    Very low-security shards could even be used for data-publishing and messaging.

??? faq "推动缩放过去n = O（c\^ 2）的独特挑战是什么？"

    There are several considerations.
    First, the algorithm would need to be converted from a two-layer algorithm to a stackable n-layer algorithm; this is possible, but is complex.
    Second, n / c (ie.
    the ratio between the total computation load of the network and the capacity of one node) is a value that happens to be close to two constants: first, if measured in blocks, a timespan of several hours, which is an acceptable “maximum security confirmation time”, and second, the ratio between rewards and deposits (an early computation suggests a 32 ETH deposit size and a 0.05 ETH block reward for Casper).
    The latter has the consequence that if rewards and penalties on a shard are escalated to be on the scale of validator deposits, the cost of continuing an attack on a shard will be O(n) in size.

    Going above c\^2 would likely entail further weakening the kinds of security guarantees that a system can provide, and allowing attackers to attack individual shards in certain ways for extended periods of time at medium cost, although it should still be possible to prevent invalid state from being finalized and to prevent finalized state from being reverted unless attackers are willing to pay an O(n) cost.
    However, the rewards are large - a super-quadratically sharded blockchain could be used as a general-purpose tool for nearly all decentralized applications, and could sustain transaction fees that makes its use virtually free.

## 脚注

1.
    Merklix tree == Merkle Patricia tree

2.
    Later proposals from the NUS group do manage to shard state; they do this via the receipt and state-compacting techniques that I describe in later sections in this document.

3.
    There are reasons to be conservative here.
    Particularly, note that if an attacker comes up with worst-case transactions whose ratio between processing time and block space expenditure (bytes, gas, etc) is much higher than usual, then the system will experience very low performance, and so a safety factor is necessary to account for this possibility.
    In traditional blockchains, the fact that block processing only takes \~1-5% of block time has the primary role of protecting against centralization risk but serves double duty of protecting against denial of service risk.
    In the specific case of Bitcoin, its current worst-case [known quadratic execution vulnerability][38] arguably limits any scaling at present to \~5-10x, and in the case of Ethereum, while all known vulnerabilities are being or have been removed after the denial-of-service attacks, there is still a risk of further discrepancies particularly on a smaller scale.
    In Bitcoin NG, the need for the former is removed, but the need for the latter is still there.

4.
    A further reason to be cautious is that increased state size corresponds to reduced throughput, as nodes will find it harder and harder to keep state data in RAM and so need more and more disk accesses, and databases, which often have an O(log(n)) access time, will take longer and longer to access.
    This was an important lesson from the last Ethereum denial-of-service attack, which bloated the state by \~10 GB by creating empty accounts and thereby indirectly slowed processing down by forcing further state accesses to hit disk instead of RAM.

5.
    In sharded blockchains, there may not necessarily be in-lockstep consensus on a single global state, and so the protocol never asks nodes to try to compute a global state root; in fact, in the protocols presented in later sections, each shard has its own state, and for each shard there is a mechanism for committing to the state root for that shard, which represents that shard’s state

6.
    #MEGA

7.
    If a non-scalable blockchain upgrades into a scalable blockchain, the author’s recommended path is that the old chain’s state should simply become a single shard in the new chain.

8.
    For this to be secure, some further conditions must be satisfied; particularly, the proof of work must be non-outsourceable in order to prevent the attacker from determining which <i>other miners' identities</i> are available for some given shard and mining on top of those.

9.
    Recent Ethereum denial-of-service attacks have proven that hard drive access is a primary bottleneck to blockchain scalability.

10.
    You could ask: well why don’t validators fetch Merkle proofs just-in-time? Answer: because doing so is a \~100-1000ms roundtrip, and executing an entire complex transaction within that time could be prohibitive.

11.
     One hybrid solution that combines the normal-case efficiency of small samples with the greater robustness of larger samples is a multi-layered sampling scheme: have a consensus between 50 nodes that requires 80% agreement to move forward, and then only if that consensus fails to be reached then fall back to a 250-node sample.
    N = 50 with an 80% threshold has only a 8.92 \* 10-9 failure rate even against attackers with p = 0.4, so this does not harm security at all under an honest or uncoordinated majority model.

12.
    The probabilities given are for one single shard; however, the random seed affects O(c) shards and the attacker could potentially take over any one of them.
    If we want to look at O(c) shards simultaneously, then there are two cases.
    First, if the grinding process is computationally bounded, then this fact does not change the calculus at all, as even though there are now O(c) chances of success per round, checking success takes O(c) times as much work.
    Second, if the grinding process is economically bounded, then this indeed calls for somewhat higher safety factors (increasing N by 10-20 should be sufficient) although it’s important to note that the goal of an attacker in a profit-motivated manipulation attack is to increase their participation across all shards in any case, and so that is the case that we are already investigating.

13.
    See [Ethcore’s Polkadotpaper][39] for further description of how their “fishermen” concept works.

14.
    Thanks to Justin Drake for pointing me to cryptographic accumulators, as well as [this paper][40] that gives the argument for the impossibility of sublinear batching.
    See also this thread: https://ethresear.ch/t/accumulators-scalability-of-utxo-blockchains-and-data-availability/176

[1]: https://web.archive.org/web/20170331105910/https://bitcoin.stackexchange.com/questions/3472/what-is-the-story-behind-the-attack-on-coiledcoin
[2]: http://www.truthcoin.info/blog/contracts-oracles-sidechains/
[3]: https://eprint.iacr.org/2017/791.pdf
[4]: https://en.wikipedia.org/wiki/Metcalfe%27s_law
[5]: https://www.comp.nus.edu.sg/~loiluu/papers/elastico.pdf
[6]: http://www.deadalnix.me/2016/11/06/using-merklix-tree-to-shard-block-validation
[7]: #ftnt_ref1
[8]: #ftnt_ref2
[9]: http://hackingdistributed.com/2015/10/14/bitcoin-ng/
[10]: #ftnt_ref3
[11]: #ftnt_ref4
[12]: https://scalingbitcoin.org/papers/mimblewimble.txt
[13]: https://github.com/ethereum/go-ethereum/pull/1889
[14]: https://github.com/paritytech/parity/wiki/Warp-Sync
[15]: https://easythereentropy.wordpress.com/2014/06/04/understanding-the-ethereum-trie
[16]: #ftnt_ref5
[17]: #ftnt_ref7
[18]: https://en.wikipedia.org/wiki/Atomicity_(database_systems
[19]: https://en.wikipedia.org/wiki/Amdahl%27s_law
[20]: https://arxiv.org/abs/1311.0243
[21]: https://blog.ethereum.org/2014/03/28/schellingcoin-a-minimal-trust-universal-data-feed/
[22]: https://blog.ethereum.org/2015/01/28/p-epsilon-attack/
[23]: https://www.reddit.com/r/Bitcoin/comments/3c305f/if_you_are_using_any_wallet_other_than_bitcoin/csrsrf9/
[24]: https://en.wikipedia.org/wiki/Binomial_distribution
[25]: https://www.microsoft.com/en-us/research/publication/permacoin-repurposing-bitcoin-work-for-data-preservation/
[26]: #ftnt_ref8
[27]: #ftnt_ref9
[28]: #ftnt_ref10
[29]: #ftnt_ref11
[30]: #ftnt_ref12
[31]: https://en.wikipedia.org/wiki/Central_limit_theorem
[32]: http://math.stackexchange.com/questions/89030/expectation-of-the-maximum-of-gaussian-random-variables
[33]: https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQ
[34]: https://arxiv.org/pdf/1406.5694.pdf
[35]: https://eprint.iacr.org/2002/081.pdf
[36]: #ftnt_ref14
[37]: https://www.youtube.com/watch?v=cU-n_m-snxQ
[38]: https://bitcoin.org/en/bitcoin-core/capacity-increases-faq#size-bump
[39]: https://github.com/polkadot-io/polkadotpaper/raw/master/PolkaDotPaper.pdf
[40]: https://eprint.iacr.org/2009/612.pdf