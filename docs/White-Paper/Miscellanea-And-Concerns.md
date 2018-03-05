# 杂记和关注

## 修改的GHOST实现

“贪婪最重观察子树”（GHOST）协议是Yonatan Sompolinsky和Aviv Zohar [2013年12月][18]首次提出的创新.
GHOST背后的动机是，由于陈旧的速度过快，确认时间快的区块链因安全性降低而遭受损失 -因为块需要一定的时间通过网络传播, 如果矿工A开采矿块，然后矿工B恰巧在矿工A的矿块传播到B之前开采另一个矿块, 矿工B的块将最终浪费，并不会有助于网络安全.
此外，还有一个集中化问题：如果矿工A是一个拥有30％哈希能力的采矿池，而B拥有10％哈希能力，那么A将有70％的时间产生陈旧块的风险（因为另外30％的时间,A产生了最后一个块，因此将立即获取挖掘数据），而B将有90％的时间产生陈旧块的风险。
因此，如果阻塞时间间隔足够短以使陈旧率较高，则仅凭借其大小
将这两种效应结合起来，快速生成区块的区块链很可能导致一个具有足够大的网络占有率的采矿池对开采过程进行事实上的控制。

正如Sompolinsky和Zohar所描述的那样，GHOST通过在计算哪个链是“最长”时包含陈旧块来解决网络安全损失的第一个问题;,也就是说，不仅仅是一个地块的父母和另外的祖先，而且还将这个地块的祖先的陈旧后裔（在以太坊术语“叔叔”中）加到计算哪个地块具有最大的完整工作证明,它。
为了解决第二个集权偏向的问题，我们超越了Sompolinsky和Zohar所描述的协议，同时也提供了区块奖励：一个陈旧块获得了其基本奖励的87.5％，包含陈旧块的侄子收到剩余,12.5％。
但是，交易费用不会发放给叔叔。

以太坊实现了GHOST的简化版本，只有七个级别。
具体来说，定义如下：

* 一个块必须指定一个父亲，并且它必须指定0个或更多的叔叔
* 包含在B区的叔叔必须具备以下特性：
  * 它必须是B的第k代祖先的直接孩子，其中2 <= k <= 7。
  * 它不能是B的祖先
  * 叔叔必须是有效的块头，但不需要是以前验证过的或甚至是有效的块
  * 一个叔叔必须和前面所有的叔叔以及同一个街区里的所有其他叔叔（非双重包含）
* 对B区的每个叔叔U而言，B的矿工获得额外的3.125％的奖励，而U的矿工获得标准体恤奖励的93.75％。

这个有限的GHOST版本只能使用7代，因为两个原因。
首先，无限制的GHOST将包含太多的复杂性，以计算给定块的哪个叔叔是有效的。
其次，在Ethereum中使用的无限制GHOST带有补偿，可以消除矿工在主链而不是公共攻击者的链条上的诱因。

## 费用

由于发布到区块链中的每一笔交易都在网络上施加了需要下载和验证的成本，所以需要一些监管机制（通常涉及交易费用）来防止滥用。
在比特币中使用的默认方法是纯粹自愿收费，依靠矿工作为守门人并设置动态最小值。
在比特币社区中，这种方法非常有利，特别是因为它是“基于市场的”，允许矿工和交易发送者之间的供求决定价格。
然而，这种推理的问题是交易处理不是市场，,尽管将交易处理作为矿工向发送方提供的服务进行解释是非常有吸引力的，但实际上矿工包括的每个交易都需要由网络中的每个节点处理，因此绝大多数交易成本,处理由第三方承担，而不是矿工决定是否将其纳入
因此，很可能发生悲剧性的共同问题。

然而，当基于市场的机制发现这个缺陷时，如果给出一个特定的不准确的简化假设，神奇地将其自行消除。
论点如下。
假设：

1. 一个交易导致“k”操作，向任何矿工提供奖励“kR”，其中包括发送方设置“R”，事先对矿工（粗略）可见的“K”和“R”。
2. 一个操作对任何节点的处理成本都是“C”（即所有节点都有相同的效率）
3. 有`N`个挖掘节点，每个节点具有完全相同的处理能力（即总数的1 / N）
4. 不存在非挖掘完整节点。

如果期望的回报高于成本，矿工会愿意处理交易。
因此，由于矿工具有处理下一个块的“1 / N”机会，期望的奖励是“kR / N”，并且处理成本
因此，矿工将包括`kR / N> kC`或`R> NC`的交易。
请注意，“R”是发件人提供的每次操作费用，因此是发件人从交易中获得的收益的下限，“NC”是整个网络一起处理操作的成本。
因此，矿工有动机只包括总功利收益超过成本的那些交易。

但是，这些假设在现实中有几个重要的偏差：

1. 矿工确实支付比其他验证节点更高的处理成本来处理交易，因为额外的验证时间延迟块传播，从而增加了块的机会将变得陈旧。
2. 确实存在nonmining全节点。
3. 采矿权分配可能在实践中最终达到极端的不平等状态。
4. 其实用功能包括对网络造成危害的投机者，政敌和疯子确实存在，而他们

（1）矿工倾向于减少交易，（2）增加“NC”;,因此，这两个效应至少部分相互抵消。
（3）和（4）是主要问题;,为了解决这些问题，我们简单地设置一个浮动上限：没有任何块比BLK_LIMIT_FACTOR乘以长期指数移动平均值更多的操作。

特别：

    blk.oplimit = floor((blk.parent.oplimit * (EMAFACTOR - 1) + floor(parent.opcount * BLK_LIMIT_FACTOR)) / EMA_FACTOR)

`BLK_LIMIT_FACTOR`和`EMA_FACTOR`是常数，暂时设置为65536和1.5，但在进一步分析后可能会改变。

还有另一个因素阻碍了比特币的大块区块：大的区块需要更长的时间才能传播，因此有更高的可能性成为比特币。
在以太坊，耗费高耗电量的块也可能花费更长的时间来传播，因为它们在物理上更大，并且花费更长的时间来处理事务状态转换以验证。
这种延迟抑制是比特币中的一个重要考虑因素，但是由于GHOST协议，在Ethereum中不那么重要。,因此，依靠监管的限额提供了一个更稳定的基线。

## 计算和图灵完整性

一个重要的注意事项是以太坊虚拟机是图灵完成;,这意味着EVM代码可以编码任何可以实现的计算，包括无限循环。
EVM代码允许以两种方式循环。
首先，有一个`JUMP`指令，允许程序跳回到代码中的前一个点，一个`JUMPI`指令做条件跳转，允许像'while x <27：x = x * 2 ,`。
其次，合同可以调用其他合约，可能允许循环递归。
这自然会导致一个问题：恶意用户是否可以通过迫使他们进入无限循环来关闭矿工和完整节点？,这个问题是由于计算机科学中存在一个问题，即停止问题而引起的：在一般情况下，无法说明给定的程序是否会停止。

如状态转换部分所述，我们的解决方案通过要求事务设置允许采用的最大计算步骤数来工作，如果执行需要更长时间的计算，则仍然支付费用。
消息以相同的方式工作。
为了展示我们解决方案背后的动机，请考虑以下示例：

* 攻击者创建一个运行无限循环的合同，然后将激活该循环的事务发送给矿工。矿工将处理交易，运行无限循环，并等待它耗尽气体。尽管执行用完了半天，交易仍然有效，矿工仍然声称。
* 攻击者创造了一个很长的无限循环，目的是迫使矿工继续计算这么长时间，计算完成后，会有更多的块出来，矿工将不可能包含交易,要求收费。
攻击者创造了一个很长的无限循环，目的是迫使矿工继续计算这么长时间，计算完成后，会有更多的块出来，矿工将不可能包含交易,要求收费。
* 攻击者看到一个类似`send（A，contract.storage [A]）的形式的代码的合同。 ,contract.storage [A] = 0`，并发送一个只有足够的气体来运行第一步但不是第二步的交易（即进行提款，但不要让余额下降）。
合同作者不需要担心防范这种攻击，因为如果执行中途停止，则变更得到恢复。
* 合同作者不需要担心防范这种攻击，因为如果执行中途停止，则变更得到恢复。
攻击者接管其中一个数据馈送，该数据馈送通过DAO部分中描述的可变地址调用机制被设计为可修改的，并将其转换为无限循环，从而试图强制任何尝试从,金融合同用完了。
但是，财务合同可以设置一个气体限制的信息，以防止这个问题。

图灵完备性的替代方法是图灵不完全性，其中不存在JUMP和JUMPI，并且在任何给定时间只允许在调用堆栈中存在每个合约的一个副本。
有了这个系统，所描述的收费制度和我们解决方案有效性的不确定性可能就没有必要，因为执行合同的成本将受到其规模的限制。
另外，图灵不完整性并不是那么大的限制;,在我们内部构想的所有合同例子中，到目前为止，只有一个需要一个循环，甚至可以通过重复一行代码来消除这个循环。
鉴于图灵完备性的严重影响以及有限的收益，为什么不简单地使用图灵不完整的语言呢？,但是，实际上，图灵不完整性并不是解决这个问题的一个好办法。

要明白为什么，请考虑以下合同：

    C0: call(C1); call(C1);
    C1: call(C2); call(C2);
    C2: call(C3); call(C3);
    ...
    C49: call(C50); call(C50);
    C50: (run one step of a program and record the change in storage)

现在，发送一个事务给A.
因此，在51笔交易中，我们有一个合约，它占用2 50个计算步骤。
矿工们可以尝试提前检测这种逻辑炸弹，方法是在每个合同旁边保留一个值，指定可以采取的最大计算步骤数，然后对递归调用其他合同的合同进行计算，但这要求矿工禁止创建合同,其他合同（因为上面所有26个合同的创建和执行可以很容易地合并成一个合同）。
另一个问题是，消息的地址字段是一个变量，所以一般情况下甚至不可能知道给定合同将提前调用哪些其他合同。
因此，总而言之，我们得到了一个令人惊讶的结论：图灵完备性出人意料地易于管理，图灵完备性的缺乏同样出人意料地难以管理，除非有相同的控制措施 - 但在这种情况下，为什么不,让协议是图灵完成？

## 货币和发行

以太坊网络包括自己的内置货币ether，它提供了一个主要的流动性层，以便在各种类型的数字资产之间进行有效的交换，更重要的是提供支付交易费用的机制。
为了方便起见，为了避免以后的争论（参见比特币当前的mBTC / uBTC / satoshi辩论），这些面额将被预先标记：

* 1: wei
* 10<sup>12</sup>: szabo
* 10<sup>15</sup>: finney
* 10<sup>18</sup>: ether

这应该被看作是“美元”和“美分”或“BTC”和“饱和”概念的扩展版本。
在不久的将来，我们预计普通交易将使用“以太网”，微交易和“szabo”使用“finney”，

发行模式如下：

* 以太网将以每个BTC 1000-2000乙醚的价格发行，这个机制旨在为以太坊组织提供资金，并支付由Mastercoin和NXT等其他平台获得成功的开发支付。
较早的买家将受益于更大的折扣。
从销售收到的BTC将完全用于支付给开发商的薪水和奖金，并投资于各种
* 总销售额的0.099x（60102216 ETH）将被分配给组织，以补偿早期的贡献者，并在创世区块之前支付以ETH计价的费用。
* 总销售金额的0.099倍作为长期储备。
* 在这一点之后，总销售额的0.26倍将永久地分配给矿工每年。

| 组           | 在发射 | 1年后  | 5年后  |
| ------------ | ------ | ------ | ------ |
| 货币单位     | 1.198X | 1.458X | 2.498X |
| 买家         | 83.5%  | 68.6%  | 40.0%  |
| 储备花了预售 | 8.26%  | 6.79%  | 3.96%  |
| 储备用于售后 | 8.26%  | 6.79%  | 3.96%  |
| 矿工         | 0%     | 17.8%  | 52.0%  |

**Long-Term Supply Growth Rate (percent)**

![SPV in bitcoin](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/inflation.png)

_尽管线性货币发行，就像比特币随着时间的推移，供应增长率趋于零_

上述模型的两个主要选择是（1）禀赋池的存在和规模，（2）存在一个永久增长的线性供给，而不是像比特币那样的供给上限。
禀赋池的理由如下。
如果禀赋池不存在，线性发行量减少到0.217x以提供相同的通货膨胀率，那么乙醚的总量将减少16.5％，因此每个单位的价值将增加19.8％。
因此，在平衡销售中将会购买19.8％以上的乙醚，因此每个单位将再次与以前一样有价值。
那么该组织也将有1.198倍的BTC，这可以被认为分为两个片：原来的比特币和额外的0.198倍。
因此，这种情况与禀赋是完全等价的，但是有一个重要的区别：组织持有纯粹的BTC，因此不支持以太单位的价值。

永久性的线性供给增长模型降低了一些人认为比特币的财富过度集中的风险，并且使得生活在现在和未来时代的个人有一个公平的机会来获得货币单位，同时保留强烈的获取和持有动机,因为随着时间的推移，“供给增长率”的百分比仍然趋于零。
我们也推断，由于粗心，死亡等原因，硬币总是会随着时间而流失，而硬币损失可以模拟为每年总供给的百分比，那么流通中的货币总量实际上最终会稳定在一个价值,等于年发行额除以损失率（例如损失率为1％，一旦供应量达到26倍，则开采0.26倍，每年损失0.26倍，创造均衡）。
请注意，未来，以太坊很可能会转而采用证券化的安全模式，将发行要求降低到每年0-0.5倍。
如果以太坊组织失去资金或者其他原因消失，我们就开放一个“社会契约”：任何人都有权创建一个未来候选版本的以太坊，唯一的条件是以太的数量必须是,最多等于`60102216 *（1.198 + 0.26 * n）`，其中`n`是发生阻塞后的年数。
创作者可以自由地出售或以其他方式分配PoS驱动的供应扩张和最大允许供应扩张之间的部分或全部差异，以支付开发费用。
不符合社会契约的候选升级可能被合理分为合规版本。

## 采矿中心

比特币挖掘算法的工作原理是让矿工在块头的稍微修改过的版本上一次又一次地计算SHA256，直到最终一个节点产生一个散列小于目标的版本 (目前在2点左右<sup>192</sup>).
但是，这种挖掘算法容易受到两种形式的集中管理。
首先，采矿生态系统已经成为ASIC（专用集成电路），计算机芯片的主导，因此专门为比特币挖掘的具体任务设计了数千倍的效率。
这意味着比特币挖掘不再是一个高度分散和平等的追求，需要数百万美元的资金才能有效参与。
其次，大多数比特币矿工实际上并没有在本地进行块验证。,相反，他们依靠集中式挖掘池来提供块头。
这个问题可以说是更糟的：截至撰写本文的时候，排名前三位的矿池间接控制了比特币网络中大约50％的处理能力，不过这可以通过矿工可以切换到其他采矿池,或联盟企图进行51％的攻击。

以太坊目前的意图是使用挖掘算法，矿工需要从状态中提取随机数据，计算区块链中最后N个块的一些随机选择的交易，并返回结果的散列。
这有两个重要的好处。
首先，以太坊合同可以包含任何类型的计算，所以以太坊专用集成电路（ASIC）本质上就是用于一般计算的专用集成电路（ASIC），即更好的CPU。
其次，采矿需要访问整个区块链，迫使矿工存储整个区块链，并至少能够验证每笔交易。
这消除了对集中式采矿池的需求;,尽管采矿池仍然可以起到平衡奖励分配随机性的合法作用，但这种功能可以通过没有中央控制的同行池同样得到很好的服务。

这个模型没有经过测试，在使用契约执行作为挖掘算法时，在避免某些巧妙的优化的过程中可能会遇到困难。
然而，这种算法的一个特别有趣的特点是，它允许任何人通过在专门设计用于阻碍某些ASIC的区块链中引入大量合同来“毒害”这个井。
ASIC制造商使用这种伎俩来攻击对方的经济诱因是存在的。
因此，我们正在开发的解决方案最终是一种适应性的经济人力解决方案，而不是单纯的技术解决方案。

## 可扩展性

以太坊常见的一个问题是可扩展性问题。
和比特币一样，以太坊也面临着网络中每个节点都需要处理每个事务的缺陷。
对于比特币，目前区块链的规模大约为15GB，每小时增长约1MB。
如果比特币网络每秒处理Visa 2000次交易，则每三秒钟将增长1MB（每小时1GB，每年8TB）。
以太坊很可能会遭遇类似的增长模式，由于在以太坊区块链之上将会有许多应用程序，而不仅仅是像比特币一样的货币，而是因为以太坊的完整节点需要存储,只是状态，而不是整个区块链历史。

这样大的区块链大小的问题是集中化风险。
如果区块链大小增加到100TB，那么可能的情况是只有极少数的大型企业会运行全部节点，所有常规用户使用轻型SPV节点。
在这种情况下，可能会出现这样的担忧，即所有的节点都可以组合在一起，并且都同意以某种有利的方式作弊（例如，改变块奖励，给自己BTC）。
光节点将无法立即检测到这一点。
当然，至少有一个诚实的完整节点可能存在，几个小时之后，关于欺诈的信息就会通过Reddit等渠道流淌出来，但是到那时候就太迟了：普通用户应该组织起来,努力将给定的区块列入黑名单，这是一个类似规模的大规模和可能的不可行的协调问题，这个问题的成功率为51％。
就比特币而言，目前这是一个问题，但是存在一个区块链修改（Peter Todd建议）[19]，这将缓解这个问题。

在短期内，以太坊将采取另外两种策略来应对这个问题。
首先，由于基于区块链的挖掘算法，至少每个矿工将被迫成为一个完整的节点，创建一个完整的节点数量的下限。
其次，更重要的是，在处理每个事务之后，我们会在区块链中包含一个中间状态树根。
即使块验证是集中式的，只要存在一个诚实的验证节点，通过验证协议就可以规避集中问题。
如果一个矿工发布了一个无效块，那么这个块必须格式化得不好，或者状态`S [n]`不正确。
由于已知`S [0]`是正确的，所以在S [i-1]是正确的时候，必须有一个第一个状态'S [i]'不正确。
验证节点将提供索引'i'，以及由需要处理'APPLY（S [i-1]，TX [i]） - > S [i]的Patricia树节点的子集组成的“无效证明” ,]`。

节点将能够使用这些节点来运行这部分计算，并且看到生成的`S[i]`与提供的`S[i]`不匹配。

另一个更复杂的攻击将涉及恶意的矿工发布不完整的块，所以完全的信息甚至不存在以确定块是否有效。
对此的解决方案是挑战 - 响应协议：验证节点以目标事务索引的形式发出“挑战”，并且在接收到节点时，光节点将该块视为不可信，直到另一节点（无论是矿工还是另一验证者）,帕特里夏节点的一个子集作为有效性的证明