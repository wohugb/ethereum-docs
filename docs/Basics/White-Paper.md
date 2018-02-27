# 白皮书

**下一代智能合约与分散式应用平台**

中本聪在2009年的比特币发展经常被誉为金钱和货币的激进发展, 是数字资产的第一个例子，它同时没有后盾或[内在价值][1]，也没有集中的发行者或控制者.

然而, 另一个 - 可以说更重要的是 - 比特币实验的一部分是底层区块链技术作为分布式共识的工具, 而关注正在迅速转移到比特币的另一个方面.

通常引用的区块链技术的替代应用包括使用区块链数字资产来表示自定义货币和金融工具(["彩色的硬币"][2]), 底层物理设备的所有权 (["智能财产"][3]), 不可互换的资产，如域名 (["Namecoin"][4]), 以及更复杂的应用程序涉及数字资产直接由一段代码执行任意规则的控制 (["智能合约"][5]) 甚至是基于区块链的 "[分散的自治组织][6]" (DAOs).

以太坊打算提供的是一个区块链，内置完全成熟的图灵完整编程语言，可用于创建“合约”，可用于编码任意状态转换功能, 允许用户创建上述任何系统, 以及其他许多我们还没有想象的事情, 只需在几行代码中编写逻辑即可.

**比特币介绍和现有的概念**

## 历史

分散式数字货币的概念以及财产登记等其他应用程序的概念已经出现了

80年代-90年代,匿名电子现金协议, 主要依赖于被称为Chaumian致盲的密码学原语, 提供了高度隐私的货币,但由于依赖中央调解机构，协议很大程度上未能获得牵引力。

1998年, 戴伟的[b-money][7]成为第一个提出通过解决计算困惑和分散共识来创造货币的想法的提案，但是关于如何实现分权化共识的细节却很少。

2005年, 哈尔·芬尼介绍了一个概念"[可重复使用的工作证明][8]", 一个使用b-money的想法与Adam Back的计算困难的Hashcash拼图一起为一个加密货币创造一个概念的系统, 但是依靠可信计算作为后端再一次没有达到理想.

2009年, 分散化的货币是中本聪实践中首次实施的, 通过公共密钥密码学把所有权管理的基本原则与一个一致的算法结合起来，以便跟踪谁拥有硬币, 被称为“工作证明”.

工作证明背后的机制是空间的突破，因为它同时解决了两个问题。

首先, 它提供了一个简单而适度的共识算法, 允许网络中的节点集体同意对比特币分类帐状态的一组规范更新.

其次, 它提供了一个允许自由进入共识进程的机制， 解决决定谁影响共识的政治问题， 同时防止sybil攻击。

这是通过取代正式的参与壁垒来实现的， 例如要求在特定名单上注册为独特实体, 带有经济障碍 - 共识投票过程中单个节点的权重与节点带来的计算能力成正比.

从那以后，提出了一种替代方法，称为_权益证明_, 计算一个节点的权重与其货币持有量成正比，而不是计算资源; 对这两种方法的相对优点的讨论超出了本文的范围，但应该指出，这两种方法都可以用作加密货币的支柱.

### 状态传递系统的比特币

![statetransition.png](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/statetransition.png)

从技术角度来看, 像比特币这样的加密货币的分类账可以被认为是一种状态转换系统, 在那里存在一个由所有现有比特币的所有权状态组成的“状态”和一个采取状态和交易的“状态转换功能”，并输出一个新的状态，这个状态是结果.

例如，在标准的银行体系中，国家是资产负债表， 交易是将`$X`从A移动到B的请求， 而状态转换函数将A账户中的价值减少`$X`，并将B账户中的价值增加`$X`。
如果A的帐户首先少于`$X`，则状态转换函数返回一个错误。
因此，可以正式定义:

    APPLY(S,TX) -> S' or ERROR

在上面定义的银行系统中:

    APPLY({ Alice: $50, Bob: $50 },"send $20 from Alice to Bob") = { Alice: $30, Bob: $70 }

但:

    APPLY({ Alice: $50, Bob: $50 },"send $70 from Alice to Bob") = ERROR

比特币中的“国家”是所有已经铸造和尚未用完的硬币(从技术上说，“未使用的交易产出”或UTXO)的集合，每个UTXO拥有一个面额和一个所有者 (由一个20字节的地址定义，该地址本质上是一个加密公钥<sup>[1]</sup>).
交易包含一个或多个输入， 每个输入包含对现有UTXO的引用和由与所有者地址关联的私钥生成的加密签名， 和一个或多个输出，每个输出包含一个新的UTXO添加到状态。

状态转换函数`APPLY(S,TX) -> S'`可以大致定义如下：

1. 对于每个输入 `TX`:
    * 如果引用的UTXO不在`S`中，则返回一个错误.
    * 如果提供的签名与UTXO的所有者不匹配，则返回错误.
2. 如果所有输入的UTXO的面值之和小于所有输出的UTXO的面值的总和, 返回一个错误.
3. 返回所有输入UTXO的`S`，并添加所有输出的UTXO.

第一步的前半部分阻止交易发件人花钱不存在的硬币, 第一步的后半部分阻止交易发件人花钱购买他人的硬币，第二步强制保值。
为了使用这个付款，协议如下。
假设Alice想把11.7 BTC发送给Bob。
首先，Alice会寻找一个她拥有的UTXO集合，总计至少为11.7 BTC。
实际上，Alice将无法获得11.7 BTC;,说她能得到的最小的是6 + 4 + 2 = 12。,然后她用这三个输入和两个输出创建一个交易。
第一个输出将是Bob的地址为11.7 BTC的所有者, 而第二个输出将是剩下的0.3比特币“变”, 主人是爱丽丝本人.

### 挖矿

![block_picture.jpg](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/block.png)

如果我们能够获得值得信赖的集中服务，那么这个系统实施起来是微不足道的; 它可以完全按照描述进行编码，使用中央服务器的硬盘来跟踪状态。
但是，对于比特币，我们正在试图建立一个分散的货币体系，所以我们需要将国家交易体系与共识体系结合起来，以确保每个人都能够就交易顺序达成一致。
比特币的分散式共识流程要求网络中的节点不断尝试生成称为“块”的交易包。
网络每十分钟产生一个块，每个块包含一个时间戳，一个随机数，一个前一个块的引用（即散列），以及自上一次以来发生的所有事务的列表块。
随着时间的推移，这将创造一个持续不断增长的“区块链”，不断更新以代表比特币分类账的最新状态。

用于检查块是否有效的算法在此范例中表示如下:

1. 检查块引用的前一个块是否存在且有效。
2. 检查块的时间戳是否大于前一个块<sup>[2]</ sup>的时间戳，并在未来不到2个小时
3. 检查块上的工作证明是否有效。
4. 让`S[0]`是前一个块末尾的状态。
5. 假设`TX`是带有`n`个事务的块的事务列表。对于`0 ... n-1`中的所有`i`，设置`S[i + 1] = APPLY（S [i]，TX [i]）`如果任何应用程序返回错误，退出并返回false。
6. 返回true，并在这个块的末尾注册`S [n]`作为状态。

本质上，块中的每个事务都必须提供一个有效的状态转换，从事务执行之前的规范状态转换到新的状态
请注意，状态不是以任何方式编码在块中;,它仅仅是一个被验证节点记住的抽象概念，并且只能通过从起始状态开始并且在每个块中顺序地应用每个事务而被（安全）地计算用于任何块。
此外，请注意，矿工包括事务处理在内的顺序。,如果在块中有两个事务A和B，使得B花费由A创建的UTXO，则如果A在B之前，则该块将是有效的，否则不是。

上述列表中有一个在其他系统中没有的有效性条件是对“工作证明”的要求。
精确的条件是，每个块的双SHA256散列，被视为一个256位的数字，必须小于一个动态调整的目标，截至本书写时约为2<sup>187</sup>.
这样做的目的是在计算上使块创建“硬”，从而防止sybil攻击者重新构建对他们有利的整个区块链。
因为SHA256被设计成一个完全不可预知的伪随机函数，所以创建一个有效的块的唯一方法就是反复试验，反复递增nonce并查看新的hash是否匹配。

在目前的〜2<sup>187</sup>, 在找到有效的块之前，网络必须进行平均2<sup>69</sup>次尝试; 一般而言，网络每隔2016年一次重新校准目标，因此平均每10分钟网络中的某个节点产生一个新的数据块.
为了补偿这个计算工作的矿工, 每个矿区的矿工有权包括一个交易给自己25 BTC无处不在.
此外，如果任何交易的投入总额高于其投入产出，则差异也作为“交易费用”交给矿工。
顺便说一下，这也是BTC发行的唯一机制。,发生的状态根本不包含任何硬币。

每个矿区的矿工有权包括一个交易给自己25 BTC无处不在
由于比特币的基础密码学已知是安全的，因此攻击者将直接针对比特币系统中不受密码学保护的部分：交易顺序。
攻击者的策略很简单：

1. 发送100 BTC到商家以换取一些产品（最好是快速交货的数字商品）
2. 等待产品交付
3. 产生另一个交易发送相同的100 BTC给自己
4. 试图说服网络，他的交易是他自己的第一个。

一旦步骤（1）发生，几分钟之后，某个矿工将把交易包括在内, 说块号码270000.大约一个小时后, 在该块之后，另外五个块将被添加到链中, 每个块都间接指向交易，从而“确认”.
此时，商家将接受付款并交付产品;,因为我们假设这是一个数字商品，交货是即时的。
现在，攻击者创建另一个发送100比特币给他自己的交易。
如果攻击者只是简单地将其释放到野外，交易将不会被处理。 矿工将尝试运行“APPLY（S，TX）”，并注意到“TX”消耗了一个不再处于该状态的UTXO。
因此，攻击者创建区块链的“分支”，首先挖掘另一个版本的区块270000，指向与父区块相同的区块269999，但新交易代替旧区块。
由于块数据不同，这需要重做工作证明。
此外，攻击者的新版块270000具有不同的散列，因此原始块270001到270005不会“指向”它;,因此，原始链条和攻击者的新链条是完全分离的。
规则是，在一个分支中，最长的区块链被认为是事实，所以合法的矿工将在270005链上工作，而攻击者只能在270000链上工作。
为了让攻击者能够使自己的区块链最长，他需要比网络其他部分更强大的计算能力来追赶（因此，“51％攻击”）。

### 默克尔树

![SPV in bitcoin](https://raw.githubusercontent.com/ethereum/www/master-postsale/src/extras/gh_wiki/spv_bitcoin.png)

_Left: 在Merkle树中只显示少量的节点就足以证明分支的有效性。_

_Right: 任何改变Merkle树的任何部分的尝试最终都会导致链上的不一致。_

比特币的一个重要的可扩展性特征是该块存储在多级数据结构中。
块的“散列”实际上只是块头的散列，一个大致200字节的数据块，包含时间戳，随机数，先前块散列和称为Merkle树的数据结构的根散列，存储所有事务,在块中。
Merkle树是一种二叉树，由一组节点组成，其中包含底层数据的树的底部有大量的叶节点，一组中间节点，其中每个节点是其两个子节点的散列，,最后是一个单一的根节点，也是由它的两个孩子的散列形成的，代表树的“顶部”。
Merkle树的目的是允许块中的数据零碎传递：一个节点只能从一个源下载块的头部，树的小部分与另一个源相关，并且仍然可靠,所有的数据都是正确的。
之所以这样做，是因为哈希会向上传播：如果恶意用户试图将虚假事务交换到Merkle树的底部，则此更改将导致上述节点发生更改，然后发生该更改,，最后改变树的根，因此改变块的散列，导致协议将它注册为完全不同的块（几乎肯定带有无效的工作证明）。

Merkle树议定书对于长期可持续性来说可能是必不可少的。
比特币网络中的一个“完整节点”，一个用于存储和处理每个区块整体的网络，截至2014年4月，在比特币网络中占用大约15 GB的磁盘空间，并且每月增长超过千兆字节。
目前，这对于一些台式计算机而不是手机是可行的，并且以后只有企业和爱好者才能参与。
称为“简化支付验证”（SPV）的协议允许存在另一类节点，称为“灯节点”，其下载块标题，验证块标题上的工作证明，然后仅下载“分支,“与与其相关的交易相关联。
这使得光节点可以确定安全性的强有力保证，比特币交易的状态和当前余额是什么，而只下载整个区块链的一小部分。

### 替代区块链应用程序

将底层区块链理念应用于其他概念的想法也具有悠久的历史。
2005年，Nick Szabo提出了“[拥有所有权的安全财产权]的概念”[9]“，该文件描述了”复制数据库技术的新进展“将如何允许基于区块链的系统存储注册表,谁拥有什么土地，创建一个精心设计的框架，包括诸如家园，逆权管理和格鲁吉亚土地税等概念。
但是，不幸的是，当时没有有效的复制数据库系统，因此该协议在实践中从未得到实施。
然而，2009年之后，一旦比特币的分散化共识得以形成，很多替代应用迅速涌现。

* **Namecoin** - 在2010年创建的[Namecoin] [10]最好被描述为一个分散的名称注册数据库。
,在像Tor，Bitcoin和BitMessage这样的分散协议中，需要有一些识别帐户的方法，以便其他人可以与它们交互，但是在所有现有的解决方案中，唯一可用的标识符是伪随机哈希，如“1LW79wp5ZBqaHW1jL5TCiBCrhQYtHagUWy”。
理想情况下，人们希望能够拥有像“乔治”这样的名字。
但问题是，如果一个人可以创建一个名为“乔治”的账户，那么其他人可以使用相同的过程为自己注册“乔治”，并假冒它们。
唯一的解决方案是首先创建的范例，第一个注册器成功，第二个失败 - 这是一个完全适合比特币共识协议的问题。
Namecoin是使用这种想法实现名称注册系统最早，也是最成功的。
* **Colored coins** - [有色金币] [2]的目的是作为一个协议，允许人们创建自己的数字货币 - 或者在一个货币与一个单位的重要微不足道的情况下，比特币区块链上的数字标记.
在彩色硬币协议中，通过公开地为特定的比特币UTXO分配一种颜色来“发布”新币种，并且该协议递归地将其他UTXO的颜色定义为与创建它们的交易花费的输入的颜色相同,（一些特殊规则适用于混合色输入的情况）.
在彩色硬币协议中，通过公开地为特定的比特币UTXO分配一种颜色来“发布”新币种，并且该协议递归地将其他UTXO的颜色定义为与创建它们的交易花费的输入的颜色相同,（一些特殊规则适用于混合色输入的情况）.
* **Metacoins** - metacoin背后的想法是拥有一个生活在比特币之上的协议，使用比特币交易来存储metacoin交易，但具有不同的状态转换功能，“APPLY”。
metacoin背后的想法是拥有一个生活在比特币之上的协议，使用比特币交易来存储metacoin交易，但具有不同的状态转换功能，“APPLY”。
这为创建任意加密货币协议提供了一种简单的机制，可能具有无法在比特币本身内部实现的高级功能，但开发成本非常低，因为挖掘和网络的复杂性已经由比特币协议处理。
Metacoins已被用于实施一些类别的金融合同，名称注册和分散交换。

因此，总的来说，有两种方法可以建立一个共识协议：建立一个独立的网络，并在比特币之上构建一个协议。
前一种方法虽然在像Namecoin这样的应用程序中合理成功，但很难实现;,每个单独的实现需要引导一个独立的区块链，以及构建和测试所有必要的状态转换和网络代码。
此外，我们预测分散式共识技术的应用程序集将遵循幂律分布，绝大多数应用程序太小而不能保证自己的区块链，并且我们注意到存在大量的分散式应用程序，特别是分散式自治,组织，需要相互交流。

另一方面，基于比特币的方法存在缺陷，即它不会继承比特币的简化支付验证功能。
SPV适用于比特币，因为它可以使用区块链深度作为有效性的代理;,在某种程度上，一旦交易的祖先足够回归，可以肯定地说它们是国家的合法组成部分。
另一方面，基于区块链的元协议不能强制区块链不包含在其自己的协议环境中无效的事务。
因此，完全安全的SPV元协议实现需要一直向后扫描到比特币区块链的开头，以确定某些交易是否有效。
目前，基于比特币的元协议的所有“轻量级”实现依赖于可信服务器来提供数据，可以说这是一个非常不理想的结果，尤其是当加密货币的主要目的之一是消除对信任的需求时。

### 脚本

即使没有任何扩展，比特币协议实际上也促成了“智能合约”概念的弱化版本。
比特币中的UTXO不仅可以拥有公钥，还可以通过以简单的基于堆栈的编程语言表达的更复杂的脚本来拥有。
在这个范例中，UTXO必须提供满足脚本的数据。
事实上，即使是基本的公钥所有权机制也是通过脚本实现的：脚本以椭圆曲线签名作为输入，根据交易和拥有UTXO的地址验证它，如果验证成功则返回1，否则返回0。
其他更复杂的脚本存在于各种其他用例中。
例如，可以构建一个脚本，该脚本需要来自给定三个私钥中的两个的签名才能验证（“multisig”），这是一种适用于公司帐户的设置，安全储蓄帐户和一些商家托管情况。
脚本也可以用来支付计算问题的解决方案，甚至可以构建一个脚本，其中写着“如果您可以提供SPV证据证明您已向此发送此币值的Dogecoin交易，则此比特币UTXO即属于您” ,，主要是允许分散的交叉加密货币交换。

但是，在比特币中实施的脚本语言有几个重要的限制：

* **Lack of Turing-completeness** - 也就是说，尽管比特币脚本语言支持大量计算，但它几乎不支持所有事情.
缺少的主要类别是循环。
这样做是为了避免交易验证期间的无限循环;,从理论上说，脚本程序员是一个可以克服的障碍，因为任何循环都可以通过简单地用if语句多次重复底层代码来模拟，但它确实会导致脚本非常空间效率低下。
例如，实施替代椭圆曲线签名算法可能需要256次重复的乘法循环，所有单独包含在代码中。
* **Value-blindness** - UTXO脚本无法提供对可撤回金额的细粒度控制。
例如，甲骨文合同的一个强大用例就是对冲合约，A和B投入1000美元的BTC，30天后脚本向A发送价值1000美元的BTC，其余的投给B.
这需要甲骨文以美元确定1 BTC的价值，但即使如此，在信任和基础设施要求方面，与现在可用的完全集中的解决方案相比，这是一个巨大的改进。
但是，由于UTXO是全有或全无，实现这一目标的唯一方式是通过非常低效地破解多个不同面值的UTXO（例如，每个k的UTXO为2 <k> </ sup>，直到,30）并且选择哪个UTXO发送给A，哪个发送给B.
* **Lack of state** - UTXO可以使用或未使用;,多阶段合同或脚本没有机会保持任何其他内部状态。
这使得很难制定多阶段期权合约，分散交易提议或两阶段密码承诺协议（安全计算奖励所必需的）。
这也意味着UTXO只能用于构建简单的一次性合同，而不是像分散组织那样更复杂的“有状态”合同，并且使元协议难以实施。
二值状态与价值失明相结合还意味着另一个重要的应用 - 撤回限制是不可能的。
* **Blockchain-blindness** - UTXO对区块链数据（例如随机数，时间戳和前一个块散列）视而不见。
,这严重限制了赌博和其他几个类别的应用，剥夺了脚本语言潜在的有价值的随机性。

因此，我们看到三种方法在加密货币之上构建高级应用程序：构建新的区块链，在比特币之上使用脚本，并在比特币之上构建元协议。
构建新的区块链可以在构建功能集时实现无限制的自由，但是需要花费开发时间，自举工作和安全性。
使用脚本很容易实现和标准化，但其功能非常有限，而元协议很容易在可伸缩性方面遇到问题。
通过以太坊，我们打算构建一个替代框架，在易于开发的同时提供更大的收益，以及更强大的轻客户端属性，同时允许应用程序共享经济环境和区块链安全。

## 以太坊

以太坊的目的是为构建分散式应用程序创建一个备用协议，提供一套不同的折衷方案，我们认为这对于大量分散式应用程序非常有用，特别强调快速开发时间，小型,很少使用的应用程序，以及不同应用程序非常有效地进行交互的能力都很重要。
以太坊通过构建本质上最终的抽象基础层来实现这一点：一种内置图灵完整编程语言的区块链，允许任何人编写智能合约和分散的应用程序，在这些应用程序中他们可以创建自己的所有权，交易格式和,状态转换功能。
Namecoin的一个简单版本可以用两行代码编写，其他协议，如货币和信誉系统可以在二十以内构建。
智能合约，包含价值并且只有在满足特定条件时才解锁的密码“箱子”也可以建立在平台之上，比比特币脚本所提供的功能强大得多，因为图灵完备性的附加功能，,价值意识，区块链认知和状态。

### 账号

在Ethereum中，状态由称为“帐户”的对象组成，每个帐户都有一个20字节的地址，状态转换是账户之间的价值和信息的直接转移。
以太坊帐户包含四个字段:

* ** nonce **，用于确保每个事务只能处理一次的计数器
* 帐户目前的**乙醚余额**
* 该帐户的**合约代码**（如果存在）
* 该帐户的**存储**（默认为空）

“以太”是以太坊的主要内部加密燃料，用于支付交易费用。
一般来说，有两种类型的账户：**由私人密钥控制的外部拥有账户**，以及由合同代码控制的**合同账户**。
外部拥有账户没有代码，并且可以通过创建和签署交易从外部拥有账户发送消息;,在合同账户中，合约账户每次收到代码激活的消息时，都允许其读取和写入内部存储并发送其他消息或依次创建合同。

请注意，以太坊中的“契约”不应被视为应该被“履行”或“遵守”的事物;,相反，他们更像是居住在以太坊执行环境中的“自主代理人”，当被消息或交易“戳穿”时总是执行特定的代码段，并直接控制他们自己的以太平衡和他们自己的密钥/,值存储以跟踪持久变量。

### 消息和交易

以太坊使用术语“交易”来指代存储从外部拥有账户发送的消息的签名数据包。
交易包含：

* The recipient of the message
* A signature identifying the sender
* The amount of ether to transfer from the sender to the recipient
* An optional data field
* A `STARTGAS` value, representing the maximum number of computational steps the transaction execution is allowed to take
* A `GASPRICE` value, representing the fee the sender pays per computational step

前三个是任何加密货币中预期的标准字段。
数据字段默认没有功能，但虚拟机具有合同可以访问数据的操作码;,作为示例用例，如果合同作为区块链域名注册服务运行，则它可能希望将传递给它的数据解释为包含两个“字段”，第一个字段是要注册的域，第二个字段是第二个字段,字段是注册它的IP地址。
合同将从消息数据中读取这些值并将其妥善放置在存储中。

对于以太坊的反拒绝服务模式，“STARTGAS”和“GASPRICE”字段至关重要。
为了防止代码中的意外或敌对的无限循环或其他计算浪费，每个事务都需要设置它可以使用的代码执行的多少计算步骤的限制。
计算的基本单位是“气”。,通常，计算步骤花费1个气体，但是一些操作耗费更高的气体量，因为它们在计算上更昂贵，或者增加了作为状态的一部分必须存储的数据量。
交易数据中的每个字节也有5个费用。
收费系统的目的是要求攻击者按比例支付他们消费的每一种资源，包括计算量，带宽和存储量;,因此，任何导致网络消耗更多这些资源的交易必须具有与增量大致成比例的燃气费。

#### 消息

合同能够将“消息”发送给其他合同。
消息是永远不会序列化的虚拟对象，只存在于以太坊执行环境中。
消息帐户

* 消息的发送者（隐式）
* 消息的接收者
* 与消息一起传输的以太量
* 可选的数据字段
* 一个“STARTGAS”值

从本质上讲，消息就像一个交易，除了它是由合同产生的而不是由外部参与者产生的。
当一个正在执行代码的合同执行产生并执行消息的`CALL`操作码时，会产生一条消息。
就像一个交易，一条消息导致收件人账户运行其代码。
因此，合约可以与外部参与者完全相同的方式与其他合约有关系。

请注意，交易或合同分配的瓦斯限额适用于该交易和所有子执行消耗的瓦斯总量。
例如，如果外部参与者A向C发送1000个气体的事务，并且B在向C发送消息之前消耗600气体，并且C的内部执行在返回之前消耗300气体，则B在运行之前可以花费另外100个气体,出气。

### 状态转移函数

![ethertransition.png](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/ethertransition.png)

以太坊状态转换函数'APPLY（S，TX） - > S''可以定义如下：

1. 以太坊状态转换函数'APPLY（S，TX） - > S''可以定义如下：
2. 将交易费用计算为“STARTGAS * GASPRICE”，并从签名确定发送地址.从发件人帐户余额中扣除费用并增加发件人的现时值。如果没有足够的余额可用，请返回错误。
3. 初始化`GAS = STARTGAS`，并且每个字节取出一定数量的气体以支付交易中的字节数。
4. 将交易金额从发件人的帐户转移到收款帐户。如果接收帐户尚不存在，请创建它。如果接收账户是合同，则运行合同代码以完成或直到执行用完为止。
5. 如果由于发件人没有足够的资金或代码执行耗尽而导致价值转移失败，请恢复除支付费用之外的所有状态更改，并将费用添加到矿工帐户。
6. 否则，将所有剩余天然气的费用退还给发送方，并将支付的天然气费用发送给矿工。

例如，假设合同的代码是：

    if !self.storage[calldataload(0)]:
        self.storage[calldataload(0)] = calldataload(32)

请注意，实际上，合同代码是用低级EVM代码编写的;,为了清楚起见，本示例使用Serpent（我们的高级语言之一）编写，并且可以编译为EVM代码。
假设合同的存储空置，并且事务以10乙醚值，2000瓦斯，0.001乙醚价格和64字节数据发送，字节0-31代表数字“2”，字节32-63代表,字符串`CHARLIE`。
在这种情况下状态转换函数的过程如下：

1. 检查交易是否有效并形成良好。
2. 检查交易发件人是否至少有2000 * 0.001 = 2以太。如果是，则从发件人帐户中减去2位以太。
3. 初始化气体= 2000;,假设交易长度为170个字节，字节费为5，则减去850，以便剩下1150个气体。
3. 从发件人账户中减去10个以上的乙醚，并将其添加到合同的账户中。
4. 运行代码,在这种情况下，这很简单：它检查是否使用了索引为'2'的合约存储，注意到它不是，因此它将索引'2'处的存储设置为'CHARLIE'值。
假设这需要187个气体，所以剩余的气体量是1150  -  187 = 963
5. 添加963 * 0.001 = 0.963乙醚返回发件人的帐户，并返回结果状态。

如果在交易的接收端没有合同，那么总的交易费用将等于所提供的`GASPRICE`乘以交易的长度（以字节为单位），并且与交易一起发送的数据将是不相关的。

请注意，消息在回复方面与事务等效：如果消息执行耗尽，那么该消息的执行以及该执行触发的所有其他执行都会恢复，但父执行不需要恢复。

这意味着合同可以调用另一份合同是“安全的”，就好像A用G气调用B一样，那么A的执行保证最多会损失G气。

最后，请注意，有一个操作码CREATE用于创建合同。,它的执行机制通常与`CALL`相似，只是执行的输出决定了新创建的合同的代码。

### 代码执行

以太坊合同中的代码采用低级，基于堆栈的字节码语言编写，称为“以太坊虚拟机代码”或“EVM代码”。
该代码由一系列字节组成，其中每个字节表示一个操作。
一般来说，代码执行是一个无限循环，包括在当前程序计数器（从零开始）重复执行操作，然后将程序计数器递增1，直到代码结束或者错误或“,检测到STOP或RETURN指令。
这些操作可以访问三种类型的存储数据的空间：

* **栈**，可以推送和弹出值的后进先出容器
* **内存**，一个无限扩展的字节数组
* 合同的长期**存储**，一个关键/价值存储。合同的长期**存储**，一个关键/价值存储。

代码还可以访问传入消息的值，发送者和数据以及块头数据，代码也可以返回一个字节数组作为输出。

EVM代码的正式执行模型非常简单。
当以太坊虚拟机正在运行时，它的完整计算状态可以由元组（`block_state，transaction，message，code，memory，stack，pc，gas）`定义，其中`block_state`是包含所有账户和,包括余额和存储。
在每一轮执行开始时，当前指令通过'code'的'pc'字节找到（或者如果'pc> = len（code）`，则为0），并且每条指令都有自己的定义,它如何影响元组。
例如，`ADD`从堆栈中弹出两件物品，并推动它们的总和，使`gas`减少1，并将`pc`增加1，`SSTORE`弹出堆栈中的前两项并将第二项插入,合同在第一项指定的索引处存储。
虽然有很多方法可以通过即时编译来优化以太坊虚拟机的执行，但以太坊的基本实现可以在几百行代码中完成。

### 区块链和挖矿

![apply_block_diagram.png](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/apply_block_diagram.png)

以太坊区块链在很多方面与比特币区块链相似，但它确实有一些区别。
以太坊和比特币在区块链架构方面的主要区别在于，与比特币不同，以太坊区块包含交易列表和最新状态的副本。
除此之外，块中还存储了其他两个值，即块号和难度。
以太坊中的基本块验证算法如下：

1. 检查前面的块引用是否存在并且是有效的。
2. 检查块的时间戳大于参考的前一个块的时间戳，并且在未来15分钟内检查
3. 检查块号，难度，交易根，叔叔根和气体限制（各种低级以太坊特定概念）是否有效。
4. 检查块的工作证明是否有效。
5. 让`S [0]`成为前一个块末尾的状态。
6. 让`TX`成为块的事务列表，其中`n`事务。
让`TX`成为块的事务列表，其中`n`事务。
如果任何应用程序返回一个错误，或者如果在该点阻塞的气体总量超过了`GASLIMIT`，则返回一个错误。
7. 让`S_FINAL`成为`S [n]`，但是增加了支付给矿工的奖励。
8. 检查状态“S_FINAL”的Merkle树根是否等于块标题中提供的最终状态根。,如果是，则该块有效;,否则，它是无效的

这种方法乍一看似乎效率很低，因为它需要在每个块中存储整个州，但实际上效率应该与比特币相当。
原因是状态存储在树状结构中，并且在每个块之后只需要改变树的一小部分。
因此，通常在两个相邻块之间绝大多数树应该是相同的，因此数据可以被存储一次并且使用指针（即子树的散列）被引用两次。
一种称为“Patricia树”的特殊树被用来实现这一点，包括对Merkle树概念的修改，允许节点被插入和删除，而不仅仅是有效地改变。
此外，由于所有状态信息都是最后一个区块的一部分，因此不需要存储整个区块链历史记录 - 如果可以应用于比特币，可以计算出的空间节省5-20倍的策略。

就物理硬件而言，一个常见问题是执行“where”合同代码。
这有一个简单的答案：执行合同代码的过程是状态转换函数定义的一部分，它是块验证算法的一部分，所以如果一个事务被添加到块“B”中，那么该事务产生的代码执行,将由现在和将来的所有节点执行，下载并验证块“B”。

## 应用

总的来说，在以太坊之上有三种类型的应用程序。
第一类是金融应用程序，为用户提供更强大的管理方式，并使用他们的资金签订合同。
这包括子货币，金融衍生品，套期保值合约，储蓄钱包，遗嘱以及最终甚至是一些类别的全面雇佣合同。
第二类是半金融应用，涉及金钱，但也有非常重要的非货币方面的工作;,一个完美的例子就是为计算问题的解决方案自我实施奖励。
最后，还有诸如在线投票和分散治理等应用程序，这些应用程序根本没有财务。

### 令牌系统

区块链上的代币系统有许多应用程序，从代表资产（如美元或黄金）的子货币到公司股票，代表智能财产的单个代币，安全不可伪造的优惠券，甚至与常规值完全无关的代币系统，用作点,激励制度。
令牌系统在Ethereum中实现起来非常简单。
要理解的关键是，所有货币或标记系统基本上都是一个数据库，只有一个操作：从A中减去X个单位并将X个单位给予B，但条件是（i）A至少有X个单位,交易和（2）交易由A批准。
实现令牌系统所需的一切就是将这个逻辑实现到合同中。

在蛇中实现令牌系统的基本代码如下所示：

    def send(to, value):
        if self.storage[msg.sender] >= value:
            self.storage[msg.sender] = self.storage[msg.sender] - value
            self.storage[to] = self.storage[to] + value

这实质上是本文件上面进一步描述的“银行系统”状态转换功能的文字实现。
需要添加一些额外的代码行以提供首先分配货币单位的初始步骤以及其他一些边缘情况，理想情况下会添加一个函数以让其他合同查询地址余额,。
但这就是它的全部。
从理论上讲，以太坊为基础的令牌系统充当子货币可能会包含链式比特币为主的多种货币所缺乏的另一个重要特征：能够直接以该货币支付交易费用。
这样做的方式是，合同将保持乙醚余额，乙醚用来向发件人支付费用，乙方会通过收取其所收取的内部货币单位来重新填充此余额，并将其转售给,一个不断运行的拍卖。
用户因此需要使用ether来“激活”他们的账户，但是一旦以太会在那里就可以重用，因为合同每次都会退还。

### 金融衍生品和稳定价值货币

金融衍生工具是“智能合约”中最常见的应用，也是最简单的代码实现之一。
实施金融合同面临的主要挑战是，其中大部分要求参考外部价格报价;,例如，一个非常理想的应用程序是一种智能合约，可以抵御以太币（或另一种加密货币）相对于美元的波动性，但这样做需要合约知道ETH / USD的价值。
最简单的方法是通过由特定方（例如NASDAQ）维护的“数据馈送”合同，以便该方有能力根据需要更新合同，并提供允许其他合同发送,向该合同发送消息并取回提供价格的响应。

鉴于这一关键因素，对冲合约看起来如下：

1. Wait for party A to input 1000 ether.
2. Wait for party B to input 1000 ether.
3. Record the USD value of 1000 ether, calculated by querying the data feed contract, in storage, say this is $x.
4. After 30 days, allow A or B to "reactivate" the contract in order to send $x worth of ether (calculated by querying the data feed contract again to get the new price) to A and the rest to B.

Such a contract would have significant potential in crypto-commerce.
One of the main problems cited about cryptocurrency is the fact that it's volatile; although many users and merchants may want the security and convenience of dealing with cryptographic assets, they may not wish to face that prospect of losing 23% of the value of their funds in a single day.
Up until now, the most commonly proposed solution has been issuer-backed assets; the idea is that an issuer creates a sub-currency in which they have the right to issue and revoke units, and provide one unit of the currency to anyone who provides them (offline) with one unit of a specified underlying asset (eg.
gold, USD).
The issuer then promises to provide one unit of the underlying asset to anyone who sends back one unit of the crypto-asset.
This mechanism allows any non-cryptographic asset to be "uplifted" into a cryptographic asset, provided that the issuer can be trusted.

In practice, however, issuers are not always trustworthy, and in some cases the banking infrastructure is too weak, or too hostile, for such services to exist.
Financial derivatives provide an alternative.
Here, instead of a single issuer providing the funds to back up an asset, a decentralized market of speculators, betting that the price of a cryptographic reference asset (eg.
ETH) will go up, plays that role.
Unlike issuers, speculators have no option to default on their side of the bargain because the hedging contract holds their funds in escrow.
Note that this approach is not fully decentralized, because a trusted source is still needed to provide the price ticker, although arguably even still this is a massive improvement in terms of reducing infrastructure requirements (unlike being an issuer, issuing a price feed requires no licenses and can likely be categorized as free speech) and reducing the potential for fraud.

### 身份和声誉系统

The earliest alternative cryptocurrency of all, [Namecoin][11], attempted to use a Bitcoin-like blockchain to provide a name registration system, where users can register their names in a public database alongside other data.
The major cited use case is for a [DNS][12] system, mapping domain names like "bitcoin.org" (or, in Namecoin's case, "bitcoin.bit") to an IP address.
Other use cases include email authentication and potentially more advanced reputation systems.
Here is the basic contract to provide a Namecoin-like name registration system on Ethereum:

    def register(name, value):
        if !self.storage[name]:
            self.storage[name] = value

The contract is very simple; all it is is a database inside the Ethereum network that can be added to, but not modified or removed from.
Anyone can register a name with some value, and that registration then sticks forever.
A more sophisticated name registration contract will also have a "function clause" allowing other contracts to query it, as well as a mechanism for the "owner" (ie.
the first registerer) of a name to change the data or transfer ownership.
One can even add reputation and web-of-trust functionality on top.

### 无心文件存储

Over the past few years, there have emerged a number of popular online file storage startups, the most prominent being Dropbox, seeking to allow users to upload a backup of their hard drive and have the service store the backup and allow the user to access it in exchange for a monthly fee.
However, at this point the file storage market is at times relatively inefficient; a cursory look at various [existing solutions][13] shows that, particularly at the "uncanny valley" 20-200 GB level at which neither free quotas nor enterprise-level discounts kick in, monthly prices for mainstream file storage costs are such that you are paying for more than the cost of the entire hard drive in a single month.
Ethereum contracts can allow for the development of a decentralized file storage ecosystem, where individual users can earn small quantities of money by renting out their own hard drives and unused space can be used to further drive down the costs of file storage.

The key underpinning piece of such a device would be what we have termed the "decentralized Dropbox contract".
This contract works as follows.
First, one splits the desired data up into blocks, encrypting each block for privacy, and builds a Merkle tree out of it.
One then makes a contract with the rule that, every N blocks, the contract would pick a random index in the Merkle tree (using the previous block hash, accessible from contract code, as a source of randomness), and give X ether to the first entity to supply a transaction with a simplified payment verification-like proof of ownership of the block at that particular index in the tree.
When a user wants to re-download their file, they can use a micropayment channel protocol (eg.
pay 1 szabo per 32 kilobytes) to recover the file; the most fee-efficient approach is for the payer not to publish the transaction until the end, instead replacing the transaction with a slightly more lucrative one with the same nonce after every 32 kilobytes.

An important feature of the protocol is that, although it may seem like one is trusting many random nodes not to decide to forget the file, one can reduce that risk down to near-zero by splitting the file into many pieces via secret sharing, and watching the contracts to see each piece is still in some node's possession.
If a contract is still paying out money, that provides a cryptographic proof that someone out there is still storing the file.

#### 无心自组织

The general concept of a "decentralized autonomous organization" is that of a virtual entity that has a certain set of members or shareholders which, perhaps with a 67% majority, have the right to spend the entity's funds and modify its code.
The members would collectively decide on how the organization should allocate its funds.
Methods for allocating a DAO's funds could range from bounties, salaries to even more exotic mechanisms such as an internal currency to reward work.
This essentially replicates the legal trappings of a traditional company or nonprofit but using only cryptographic blockchain technology for enforcement.
So far much of the talk around DAOs has been around the "capitalist" model of a "decentralized autonomous corporation" (DAC) with dividend-receiving shareholders and tradable shares; an alternative, perhaps described as a "decentralized autonomous community", would have all members have an equal share in the decision making and require 67% of existing members to agree to add or remove a member.
The requirement that one person can only have one membership would then need to be enforced collectively by the group.

A general outline for how to code a DAO is as follows.
The simplest design is simply a piece of self-modifying code that changes if two thirds of members agree on a change.
Although code is theoretically immutable, one can easily get around this and have de-facto mutability by having chunks of the code in separate contracts, and having the address of which contracts to call stored in the modifiable storage.
In a simple implementation of such a DAO contract, there would be three transaction types, distinquished by the data provided in the transaction:

* `[0,i,K,V]` to register a proposal with index `i` to change the address at storage index `K` to value `V`
* `[0,i]` to register a vote in favor of proposal `i`
* `[2,i]` to finalize proposal `i` if enough votes have been made

The contract would then have clauses for each of these.
It would maintain a record of all open storage changes, along with a list of who voted for them.
It would also have a list of all members.
When any storage change gets to two thirds of members voting for it, a finalizing transaction could execute the change.
A more sophisticated skeleton would also have built-in voting ability for features like sending a transaction, adding members and removing members, and may even provide for [Liquid Democracy][14]-style vote delegation (ie.
anyone can assign someone to vote for them, and assignment is transitive so if A assigns B and B assigns C then C determines A's vote).
This design would allow the DAO to grow organically as a decentralized community, allowing people to eventually delegate the task of filtering out who is a member to specialists, although unlike in the "current system" specialists can easily pop in and out of existence over time as individual community members change their alignments.

An alternative model is for a decentralized corporation, where any account can have zero or more shares, and two thirds of the shares are required to make a decision.
A complete skeleton would involve asset management functionality, the ability to make an offer to buy or sell shares, and the ability to accept offers (preferably with an order-matching mechanism inside the contract).
Delegation would also exist Liquid Democracy-style, generalizing the concept of a "board of directors".

### 其它应用

**1. Savings wallets**.
Suppose that Alice wants to keep her funds safe, but is worried that she will lose or someone will hack her private key.
She puts ether into a contract with Bob, a bank, as follows:

* Alice alone can withdraw a maximum of 1% of the funds per day.
* Bob alone can withdraw a maximum of 1% of the funds per day, but Alice has the ability to make a transaction with her key shutting off this ability.
* Alice and Bob together can withdraw anything.

Normally, 1% per day is enough for Alice, and if Alice wants to withdraw more she can contact Bob for help.
If Alice's key gets hacked, she runs to Bob to move the funds to a new contract.
If she loses her key, Bob will get the funds out eventually.
If Bob turns out to be malicious, then she can turn off his ability to withdraw.

**2. Crop insurance**.
One can easily make a financial derivatives contract but using a data feed of the weather instead of any price index.
If a farmer in Iowa purchases a derivative that pays out inversely based on the precipitation in Iowa, then if there is a drought, the farmer will automatically receive money and if there is enough rain the farmer will be happy because their crops would do well.
This can be expanded to natural disaster insurance generally.

**3. A decentralized data feed**.
For financial contracts for difference, it may actually be possible to decentralize the data feed via a protocol called "[SchellingCoin][15]".
SchellingCoin basically works as follows: N parties all put into the system the value of a given datum (eg.
the ETH/USD price), the values are sorted, and everyone between the 25th and 75th percentile gets one token as a reward.
Everyone has the incentive to provide the answer that everyone else will provide, and the only value that a large number of players can realistically agree on is the obvious default: the truth.
This creates a decentralized protocol that can theoretically provide any number of values, including the ETH/USD price, the temperature in Berlin or even the result of a particular hard computation.

**4. Smart multisignature escrow**.
Bitcoin allows multisignature transaction contracts where, for example, three out of a given five keys can spend the funds.
Ethereum allows for more granularity; for example, four out of five can spend everything, three out of five can spend up to 10% per day, and two out of five can spend up to 0.5% per day.
Additionally, Ethereum multisig is asynchronous - two parties can register their signatures on the blockchain at different times and the last signature will automatically send the transaction.

**5. Cloud computing**.
The EVM technology can also be used to create a verifiable computing environment, allowing users to ask others to carry out computations and then optionally ask for proofs that computations at certain randomly selected checkpoints were done correctly.
This allows for the creation of a cloud computing market where any user can participate with their desktop, laptop or specialized server, and spot-checking together with security deposits can be used to ensure that the system is trustworthy (ie.
nodes cannot profitably cheat).
Although such a system may not be suitable for all tasks; tasks that require a high level of inter-process communication, for example, cannot easily be done on a large cloud of nodes.
Other tasks, however, are much easier to parallelize; projects like SETI@home, folding@home and genetic algorithms can easily be implemented on top of such a platform.

**6. Peer-to-peer gambling**.
Any number of peer-to-peer gambling protocols, such as Frank Stajano and Richard Clayton's [Cyberdice][16], can be implemented on the Ethereum blockchain.
The simplest gambling protocol is actually simply a contract for difference on the next block hash, and more advanced protocols can be built up from there, creating gambling services with near-zero fees that have no ability to cheat.

**7. Prediction markets**.
Provided an oracle or SchellingCoin, prediction markets are also easy to implement, and prediction markets together with SchellingCoin may prove to be the first mainstream application of [futarchy][17] as a governance protocol for decentralized organizations.

**8. On-chain decentralized marketplaces**, using the identity and reputation system as a base.

## 杂记和关注

### 修改的GHOST实现

The "Greedy Heaviest Observed Subtree" (GHOST) protocol is an innovation first introduced by Yonatan Sompolinsky and Aviv Zohar in [December 2013][18].
The motivation behind GHOST is that blockchains with fast confirmation times currently suffer from reduced security due to a high stale rate - because blocks take a certain time to propagate through the network, if miner A mines a block and then miner B happens to mine another block before miner A's block propagates to B, miner B's block will end up wasted and will not contribute to network security.
Furthermore, there is a centralization issue: if miner A is a mining pool with 30% hashpower and B has 10% hashpower, A will have a risk of producing a stale block 70% of the time (since the other 30% of the time A produced the last block and so will get mining data immediately) whereas B will have a risk of producing a stale block 90% of the time.
Thus, if the block interval is short enough for the stale rate to be high, A will be substantially more efficient simply by virtue of its size.
With these two effects combined, blockchains which produce blocks quickly are very likely to lead to one mining pool having a large enough percentage of the network hashpower to have de facto control over the mining process.

As described by Sompolinsky and Zohar, GHOST solves the first issue of network security loss by including stale blocks in the calculation of which chain is the "longest"; that is to say, not just the parent and further ancestors of a block, but also the stale descendants of the block's ancestor (in Ethereum jargon, "uncles") are added to the calculation of which block has the largest total proof of work backing it.
To solve the second issue of centralization bias, we go beyond the protocol described by Sompolinsky and Zohar, and also provide block rewards to stales: a stale block receives 87.5% of its base reward, and the nephew that includes the stale block receives the remaining 12.5%.
Transaction fees, however, are not awarded to uncles.

Ethereum implements a simplified version of GHOST which only goes down seven levels.
Specifically, it is defined as follows:

* A block must specify a parent, and it must specify 0 or more uncles
* An uncle included in block B must have the following properties:
  * It must be a direct child of the kth generation ancestor of B, where 2 <= k <= 7.
  * It cannot be an ancestor of B
  * An uncle must be a valid block header, but does not need to be a previously verified or even valid block
  * An uncle must be different from all uncles included in previous blocks and all other uncles included in the same block (non-double-inclusion)
* For every uncle U in block B, the miner of B gets an additional 3.125% added to its coinbase reward and the miner of U gets 93.75% of a standard coinbase reward.

This limited version of GHOST, with uncles includable only up to 7 generations, was used for two reasons.
First, unlimited GHOST would include too many complications into the calculation of which uncles for a given block are valid.
Second, unlimited GHOST with compensation as used in Ethereum removes the incentive for a miner to mine on the main chain and not the chain of a public attacker.

### 费用

Because every transaction published into the blockchain imposes on the network the cost of needing to download and verify it, there is a need for some regulatory mechanism, typically involving transaction fees, to prevent abuse.
The default approach, used in Bitcoin, is to have purely voluntary fees, relying on miners to act as the gatekeepers and set dynamic minimums.
This approach has been received very favorably in the Bitcoin community particularly because it is "market-based", allowing supply and demand between miners and transaction senders determine the price.
The problem with this line of reasoning is, however, that transaction processing is not a market; although it is intuitively attractive to construe transaction processing as a service that the miner is offering to the sender, in reality every transaction that a miner includes will need to be processed by every node in the network, so the vast majority of the cost of transaction processing is borne by third parties and not the miner that is making the decision of whether or not to include it.
Hence, tragedy-of-the-commons problems are very likely to occur.

However, as it turns out this flaw in the market-based mechanism, when given a particular inaccurate simplifying assumption, magically cancels itself out.
The argument is as follows.
Suppose that:

1. A transaction leads to `k` operations, offering the reward `kR` to any miner that includes it where `R` is set by the sender and `k` and `R` are (roughly) visible to the miner beforehand.
2. An operation has a processing cost of `C` to any node (ie.
all nodes have equal efficiency)
3. There are `N` mining nodes, each with exactly equal processing power (ie.
`1/N` of total)
4. No non-mining full nodes exist.

A miner would be willing to process a transaction if the expected reward is greater than the cost.
Thus, the expected reward is `kR/N` since the miner has a `1/N` chance of processing the next block, and the processing cost for the miner is simply `kC`.
Hence, miners will include transactions where `kR/N > kC`, or `R > NC`.
Note that `R` is the per-operation fee provided by the sender, and is thus a lower bound on the benefit that the sender derives from the transaction, and `NC` is the cost to the entire network together of processing an operation.
Hence, miners have the incentive to include only those transactions for which the total utilitarian benefit exceeds the cost.

However, there are several important deviations from those assumptions in reality:

1. The miner does pay a higher cost to process the transaction than the other verifying nodes, since the extra verification time delays block propagation and thus increases the chance the block will become a stale.
2. There do exist nonmining full nodes.
3. The mining power distribution may end up radically inegalitarian in practice.
4. Speculators, political enemies and crazies whose utility function includes causing harm to the network do exist, and they can cleverly set up contracts where their cost is much lower than the cost paid by other verifying nodes.

(1) provides a tendency for the miner to include fewer transactions, and (2) increases `NC`; hence, these two effects at least partially cancel each other out.
(3) and (4) are the major issue; to solve them we simply institute a floating cap: no block can have more operations than `BLK_LIMIT_FACTOR` times the long-term exponential moving average.
Specifically:

    blk.oplimit = floor((blk.parent.oplimit * (EMAFACTOR - 1) + floor(parent.opcount * BLK_LIMIT_FACTOR)) / EMA_FACTOR)

`BLK_LIMIT_FACTOR` and `EMA_FACTOR` are constants that will be set to 65536 and 1.5 for the time being, but will likely be changed after further analysis.

There is another factor disincentivizing large block sizes in Bitcoin: blocks that are large will take longer to propagate, and thus have a higher probability of becoming stales.
In Ethereum, highly gas-consuming blocks can also take longer to propagate both because they are physically larger and because they take longer to process the transaction state transitions to validate.
This delay disincentive is a significant consideration in Bitcoin, but less so in Ethereum because of the GHOST protocol; hence, relying on regulated block limits provides a more stable baseline.

### 计算和图灵完整性

An important note is that the Ethereum virtual machine is Turing-complete; this means that EVM code can encode any computation that can be conceivably carried out, including infinite loops.
EVM code allows looping in two ways.
First, there is a `JUMP` instruction that allows the program to jump back to a previous spot in the code, and a `JUMPI` instruction to do conditional jumping, allowing for statements like `while x < 27: x = x * 2`.
Second, contracts can call other contracts, potentially allowing for looping through recursion.
This naturally leads to a problem: can malicious users essentially shut miners and full nodes down by forcing them to enter into an infinite loop? The issue arises because of a problem in computer science known as the halting problem: there is no way to tell, in the general case, whether or not a given program will ever halt.

As described in the state transition section, our solution works by requiring a transaction to set a maximum number of computational steps that it is allowed to take, and if execution takes longer computation is reverted but fees are still paid.
Messages work in the same way.
To show the motivation behind our solution, consider the following examples:

* An attacker creates a contract which runs an infinite loop, and then sends a transaction activating that loop to the miner.
The miner will process the transaction, running the infinite loop, and wait for it to run out of gas.
Even though the execution runs out of gas and stops halfway through, the transaction is still valid and the miner still claims the fee from the attacker for each computational step.
* An attacker creates a very long infinite loop with the intent of forcing the miner to keep computing for such a long time that by the time computation finishes a few more blocks will have come out and it will not be possible for the miner to include the transaction to claim the fee.
However, the attacker will be required to submit a value for `STARTGAS` limiting the number of computational steps that execution can take, so the miner will know ahead of time that the computation will take an excessively large number of steps.
* An attacker sees a contract with code of some form like `send(A,contract.storage[A]); contract.storage[A] = 0`, and sends a transaction with just enough gas to run the first step but not the second (ie.
making a withdrawal but not letting the balance go down).
The contract author does not need to worry about protecting against such attacks, because if execution stops halfway through the changes get reverted.
* A financial contract works by taking the median of nine proprietary data feeds in order to minimize risk.
An attacker takes over one of the data feeds, which is designed to be modifiable via the variable-address-call mechanism described in the section on DAOs, and converts it to run an infinite loop, thereby attempting to force any attempts to claim funds from the financial contract to run out of gas.
However, the financial contract can set a gas limit on the message to prevent this problem.

The alternative to Turing-completeness is Turing-incompleteness, where `JUMP` and `JUMPI` do not exist and only one copy of each contract is allowed to exist in the call stack at any given time.
With this system, the fee system described and the uncertainties around the effectiveness of our solution might not be necessary, as the cost of executing a contract would be bounded above by its size.
Additionally, Turing-incompleteness is not even that big a limitation; out of all the contract examples we have conceived internally, so far only one required a loop, and even that loop could be removed by making 26 repetitions of a one-line piece of code.
Given the serious implications of Turing-completeness, and the limited benefit, why not simply have a Turing-incomplete language? In reality, however, Turing-incompleteness is far from a neat solution to the problem.
To see why, consider the following contracts:

    C0: call(C1); call(C1);
    C1: call(C2); call(C2);
    C2: call(C3); call(C3);
    ...
    C49: call(C50); call(C50);
    C50: (run one step of a program and record the change in storage)

Now, send a transaction to A.
Thus, in 51 transactions, we have a contract that takes up 2<sup>50</sup> computational steps.
Miners could try to detect such logic bombs ahead of time by maintaining a value alongside each contract specifying the maximum number of computational steps that it can take, and calculating this for contracts calling other contracts recursively, but that would require miners to forbid contracts that create other contracts (since the creation and execution of all 26 contracts above could easily be rolled into a single contract).
Another problematic point is that the address field of a message is a variable, so in general it may not even be possible to tell which other contracts a given contract will call ahead of time.
Hence, all in all, we have a surprising conclusion: Turing-completeness is surprisingly easy to manage, and the lack of Turing-completeness is equally surprisingly difficult to manage unless the exact same controls are in place - but in that case why not just let the protocol be Turing-complete?

### 货币和发行

The Ethereum network includes its own built-in currency, ether, which serves the dual purpose of providing a primary liquidity layer to allow for efficient exchange between various types of digital assets and, more importantly, of providing a mechanism for paying transaction fees.
For convenience and to avoid future argument (see the current mBTC/uBTC/satoshi debate in Bitcoin), the denominations will be pre-labelled:

* 1: wei
* 10<sup>12</sup>: szabo
* 10<sup>15</sup>: finney
* 10<sup>18</sup>: ether

This should be taken as an expanded version of the concept of "dollars" and "cents" or "BTC" and "satoshi".
In the near future, we expect "ether" to be used for ordinary transactions, "finney" for microtransactions and "szabo" and "wei" for technical discussions around fees and protocol implementation; the remaining denominations may become useful later and should not be included in clients at this point.

The issuance model will be as follows:

* Ether will be released in a currency sale at the price of 1000-2000 ether per BTC, a mechanism intended to fund the Ethereum organization and pay for development that has been used with success by other platforms such as Mastercoin and NXT.
Earlier buyers will benefit from larger discounts.
The BTC received from the sale will be used entirely to pay salaries and bounties to developers and invested into various for-profit and non-profit projects in the Ethereum and cryptocurrency ecosystem.
* 0.099x the total amount sold (60102216 ETH) will be allocated to the organization to compensate early contributors and pay ETH-denominated expenses before the genesis block.
* 0.099x the total amount sold will be maintained as a long-term reserve.
* 0.26x the total amount sold will be allocated to miners per year forever after that point.

| Group  | At launch | After 1 year | After 5 years
| ------------- | ------------- |-------------| ----------- |
| Currency units  | 1.198X | 1.458X  |  2.498X |
| Purchasers  | 83.5% | 68.6%  | 40.0% |
| Reserve spent pre-sale | 8.26% | 6.79% | 3.96% |
| Reserve used post-sale | 8.26% | 6.79% | 3.96% |
| Miners | 0% | 17.8% | 52.0% |

**Long-Term Supply Growth Rate (percent)**

![SPV in bitcoin](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/inflation.png)

_Despite the linear currency issuance, just like with Bitcoin over time the supply growth rate nevertheless tends to zero_

The two main choices in the above model are (1) the existence and size of an endowment pool, and (2) the existence of a permanently growing linear supply, as opposed to a capped supply as in Bitcoin.
The justification of the endowment pool is as follows.
If the endowment pool did not exist, and the linear issuance reduced to 0.217x to provide the same inflation rate, then the total quantity of ether would be 16.5% less and so each unit would be 19.8% more valuable.
Hence, in the equilibrium 19.8% more ether would be purchased in the sale, so each unit would once again be exactly as valuable as before.
The organization would also then have 1.198x as much BTC, which can be considered to be split into two slices: the original BTC, and the additional 0.198x.
Hence, this situation is _exactly equivalent_ to the endowment, but with one important difference: the organization holds purely BTC, and so is not incentivized to support the value of the ether unit.

The permanent linear supply growth model reduces the risk of what some see as excessive wealth concentration in Bitcoin, and gives individuals living in present and future eras a fair chance to acquire currency units, while at the same time retaining a strong incentive to obtain and hold ether because the "supply growth rate" as a percentage still tends to zero over time.
We also theorize that because coins are always lost over time due to carelessness, death, etc, and coin loss can be modeled as a percentage of the total supply per year, that the total currency supply in circulation will in fact eventually stabilize at a value equal to the annual issuance divided by the loss rate (eg.
at a loss rate of 1%, once the supply reaches 26X then 0.26X will be mined and 0.26X lost every year, creating an equilibrium).

Note that in the future, it is likely that Ethereum will switch to a proof-of-stake model for security, reducing the issuance requirement to somewhere between zero and 0.05X per year.
In the event that the Ethereum organization loses funding or for any other reason disappears, we leave open a "social contract": anyone has the right to create a future candidate version of Ethereum, with the only condition being that the quantity of ether must be at most equal to `60102216 * (1.198 + 0.26 * n)` where `n` is the number of years after the genesis block.
Creators are free to crowd-sell or otherwise assign some or all of the difference between the PoS-driven supply expansion and the maximum allowable supply expansion to pay for development.
Candidate upgrades that do not comply with the social contract may justifiably be forked into compliant versions.

### 采矿中心

The Bitcoin mining algorithm works by having miners compute SHA256 on slightly modified versions of the block header millions of times over and over again, until eventually one node comes up with a version whose hash is less than the target (currently around 2<sup>192</sup>).
However, this mining algorithm is vulnerable to two forms of centralization.
First, the mining ecosystem has come to be dominated by ASICs (application-specific integrated circuits), computer chips designed for, and therefore thousands of times more efficient at, the specific task of Bitcoin mining.
This means that Bitcoin mining is no longer a highly decentralized and egalitarian pursuit, requiring millions of dollars of capital to effectively participate in.
Second, most Bitcoin miners do not actually perform block validation locally; instead, they rely on a centralized mining pool to provide the block headers.
This problem is arguably worse: as of the time of this writing, the top three mining pools indirectly control roughly 50% of processing power in the Bitcoin network, although this is mitigated by the fact that miners can switch to other mining pools if a pool or coalition attempts a 51% attack.

The current intent at Ethereum is to use a mining algorithm where miners are required to fetch random data from the state, compute some randomly selected transactions from the last N blocks in the blockchain, and return the hash of the result.
This has two important benefits.
First, Ethereum contracts can include any kind of computation, so an Ethereum ASIC would essentially be an ASIC for general computation - ie.
a better CPU.
Second, mining requires access to the entire blockchain, forcing miners to store the entire blockchain and at least be capable of verifying every transaction.
This removes the need for centralized mining pools; although mining pools can still serve the legitimate role of evening out the randomness of reward distribution, this function can be served equally well by peer-to-peer pools with no central control.

This model is untested, and there may be difficulties along the way in avoiding certain clever optimizations when using contract execution as a mining algorithm.
However, one notably interesting feature of this algorithm is that it allows anyone to "poison the well", by introducing a large number of contracts into the blockchain specifically designed to stymie certain ASICs.
The economic incentives exist for ASIC manufacturers to use such a trick to attack each other.
Thus, the solution that we are developing is ultimately an adaptive economic human solution rather than purely a technical one.

### 可扩展性

One common concern about Ethereum is the issue of scalability.
Like Bitcoin, Ethereum suffers from the flaw that every transaction needs to be processed by every node in the network.
With Bitcoin, the size of the current blockchain rests at about 15 GB, growing by about 1 MB per hour.
If the Bitcoin network were to process Visa's 2000 transactions per second, it would grow by 1 MB per three seconds (1 GB per hour, 8 TB per year).
Ethereum is likely to suffer a similar growth pattern, worsened by the fact that there will be many applications on top of the Ethereum blockchain instead of just a currency as is the case with Bitcoin, but ameliorated by the fact that Ethereum full nodes need to store just the state instead of the entire blockchain history.

The problem with such a large blockchain size is centralization risk.
If the blockchain size increases to, say, 100 TB, then the likely scenario would be that only a very small number of large businesses would run full nodes, with all regular users using light SPV nodes.
In such a situation, there arises the potential concern that the full nodes could band together and all agree to cheat in some profitable fashion (eg.
change the block reward, give themselves BTC).
Light nodes would have no way of detecting this immediately.
Of course, at least one honest full node would likely exist, and after a few hours information about the fraud would trickle out through channels like Reddit, but at that point it would be too late: it would be up to the ordinary users to organize an effort to blacklist the given blocks, a massive and likely infeasible coordination problem on a similar scale as that of pulling off a successful 51% attack.
In the case of Bitcoin, this is currently a problem, but there exists a blockchain modification [suggested by Peter Todd][19] which will alleviate this issue.

In the near term, Ethereum will use two additional strategies to cope with this problem.
First, because of the blockchain-based mining algorithms, at least every miner will be forced to be a full node, creating a lower bound on the number of full nodes.
Second and more importantly, however, we will include an intermediate state tree root in the blockchain after processing each transaction.
Even if block validation is centralized, as long as one honest verifying node exists, the centralization problem can be circumvented via a verification protocol.
If a miner publishes an invalid block, that block must either be badly formatted, or the state `S[n]` is incorrect.
Since `S[0]` is known to be correct, there must be some first state `S[i]` that is incorrect where `S[i-1]` is correct.
The verifying node would provide the index `i`, along with a "proof of invalidity" consisting of the subset of Patricia tree nodes needing to process `APPLY(S[i-1],TX[i]) -> S[i]`.

节点将能够使用这些节点来运行这部分计算，并且看到生成的`S[i]`与提供的`S[i]`不匹配。

另一个更复杂的攻击将涉及恶意的矿工发布不完整的块，所以完全的信息甚至不存在以确定块是否有效。
对此的解决方案是挑战 - 响应协议：验证节点以目标事务索引的形式发出“挑战”，并且在接收到节点时，光节点将该块视为不可信，直到另一节点（无论是矿工还是另一验证者）,帕特里夏节点的一个子集作为有效性的证明

## 结论

以太坊协议最初被设想为一种加密货币的升级版本, 提供高级功能，如区块链托管，取款限额, 金融合同，赌博市场等等，通过高度概括的编程语言.
以太坊协议不会直接“支持”任何应用程序, 但是图灵完全编程语言的存在意味着可以在理论上为任何交易类型或应用程序创建任意合约.
然而，以太坊更有趣的是以太坊协议远远超出了货币。
有关分散文件存储的协议, 分散计算和分散预测市场, 在其他几十个这样的概念之中，有可能大大提高计算行业的效率, 并通过首次增加一个经济层来大幅提升其他对等协议.
最后，还有大量与金钱无关的应用程序.

由以太坊协议实施的任意状态转换功能的概念提供了一个具有独特潜力的平台; `以太坊`并不是一个封闭式的单一用途协议，专为数据存储，赌博或金融领域的特定应用程序而设计，我们相信它非常适合作为基础,在未来几年将会有大量的财务和非财务协议。

## 笔记和进一步阅读

### 笔记

1. 一个复杂的读者可能会注意到，实际上比特币地址是椭圆曲线公钥的散列，而不是公钥本身。然而，它实际上是完全合法的密码术语，指的是公钥本身作为公钥。这是因为比特币的密码学可以被认为是一种定制的数字签名算法，其中公钥由ECC公钥的哈希组成，签名由与ECC签名串联的ECC公钥组成，并且验证算法涉及检查ECC,签名中的pubkey与作为公钥提供的ECC pubkey散列签名，然后根据ECC pubkey验证ECC签名。
2. 技术上，前11个区块的中位数。
3. 在内部，2和“CHARLIE”都是数字，后者是以大端基数256表示。数字可以是至少0和最多2<sup>256</sup>-1.

### 进一步阅读

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
[4]: http://namecoin.org
[5]: http://szabo.best.vwh.net/smart_contracts_idea.html
[6]: http://bitcoinmagazine.com/7050/bootstrapping-a-decentralized-autonomous-corporation-part-i/
[7]: http://www.weidai.com/bmoney.txt
[8]: http://nakamotoinstitute.org/finney/rpow/
[9]: http://szabo.best.vwh.net/securetitle.html
[10]: https://namecoin.org/
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