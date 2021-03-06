# 以太坊

以太坊的目的是为构建分散式应用程序创建一个备用协议，提供一套不同的折衷方案，我们认为这对于大量的分散式应用程序非常有用，特别强调快速开发时间，小型,很少使用的应用程序，以及不同应用程序非常有效地交互的能力都很重要。
以太坊通过构建本质上最终的抽象基础层来实现这一点：一个内置图灵完整编程语言的区块链，允许任何人编写智能合约和分散的应用程序，在那里他们可以创建自己的所有权，交易格式和,状态转换功能。
Namecoin的一个简单的版本可以用两行代码来编写，像货币和信誉系统这样的其他协议可以在二十几年之内建立起来。
智能合约，包含价值的密码“盒子”，只有在某些条件得到满足的情况下才能解锁，也可以建立在平台之上，比比特币脚本更强大，因为图灵完备性更强，,价值意识，区块链认知和状态。

## 账号

在Ethereum中，状态由称为“账户”的对象组成，每个账户具有20字节的地址，并且状态转换是账户之间的价值和信息的直接转移。
以太坊账户包含四个字段：

* ** nonce **，用于确保每个事务只能处理一次的计数器
* 帐户目前的**以太平衡**
* 该帐户的**合约代码**（如果存在）
* 该帐户的**存储**（默认为空）

“以太”是以太坊的主要内部加密燃料，用于支付交易费用。
一般来说，有两种类型的账户：**由私人密钥控制的外部拥有账户**，以及由合同代码控制的**合同账户**。
外部拥有的账户没有代码，并且可以通过创建和签署交易来从外部拥有账户发送消息;,在合同账户中，合同账户每收到一条消息，代码就会激活，允许其读取和写入内部存储并发送其他消息或依次创建合同。

请注意，以太坊中的“契约”不应被视为应该被“履行”或“遵守”的东西。,相反，它们更像是以太坊执行环境中的“自主代理”，当被消息或事务“戳穿”时总是执行特定的代码段，并直接控制自己的以太平衡和自己的密钥/,值存储来跟踪持久变量。

## 消息和交易

以太坊使用术语“交易”来指代存储从外部拥有账户发送的消息的签名数据包。
交易包含:

* 消息的收件人
* 识别发件人的签名
* 从发件人到收件人的乙醚数量
* 可选的数据字段
* 一个“STARTGAS”值，表示允许执行事务的最大计算步骤数
* 一个“STARTGAS”值，表示允许执行事务的最大计算步骤数

前三个是任何加密货币中预期的标准字段。
数据字段默认没有任何功能，但是虚拟机有一个合同可以访问数据的操作码;,作为示例用例，如果一个合同作为区块链上的域名注册服务运行，那么它可能希望将传递给它的数据解释为包含两个“字段”，第一个字段是要注册的域，第二个字段,字段是将其注册到的IP地址。
合同将从消息数据中读取这些值，并将其妥善存放。

对于以太坊的反拒绝服务模式，“STARTGAS”和“GASPRICE”字段是至关重要的。
为了防止代码中的意外或敌对的无限循环或其他计算浪费，每个事务都需要设置它可以使用的代码执行的多少计算步骤的限制。
计算的基本单位是“气”。,通常，计算步骤花费1个气体，但是一些操作花费更高的气体量，因为它们在计算上更昂贵，或者增加了作为状态的一部分必须存储的数据量。交易数据中的每个字节也有5个天然气的费用。
收费制度的目的是要求攻击者按比例支付他们消费的所有资源，包括计算量，带宽和存储量;,因此，任何导致网络消耗更多这些资源的交易必须具有与增量大致成比例的燃气费。

### 消息

合同能够将“消息”发送给其他合同。
消息是从不序列化的虚拟对象，只存在于以太坊执行环境中。
消息包含：

* 消息的发送者（隐式）
* 消息的收件人
* 与消息一起传输的以太量
* 可选的数据字段
* 一个“STARTGAS”值

从本质上讲，消息就像一个交易，除了它是由合同而不是由外部参与者产生的。
当一个正在执行代码的协议执行`CALL`操作码时，产生一个消息，该操作码产生和执行一个消息
就像一个交易，一条消息导致收件人帐户运行其代码。
因此，契约可以和其他契约的关系完全一样，外部行为者可以。

请注意，交易或合同分配的瓦斯补贴适用于该交易和所有子执行消耗的瓦斯总量。
例如，如果一个外部参与者A向C发送1000个事务，而B向C发送消息之前消耗600个气体，并且C的内部执行在返回之前消耗300个气体，那么B在运行之前可以花费另外100个气体,出气。

## 状态转移函数

![ethertransition.png](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/ethertransition.png)

以太坊状态转换函数'APPLY（S，TX） - > S''可以定义如下：

1. 检查交易格式是否正确（即，具有正确的值数量），签名是否有效，以及随机数是否与发件人帐户中的现时值相匹配。如果没有，则返回错误。
2. 计算交易费用为“STARTGAS * GASPRICE”，并从签名确定发送地址。从发件人帐户余额中扣除费用，并增加发件人的现时值。如果没有足够的余额花费，返回一个错误。
3. 初始化`GAS = STARTGAS`，并且每个字节取出一定数量的气体来支付交易中的字节数。
4. 将交易金额从发件人的帐户转移到收款帐户。如果接收帐户不存在，请创建它。如果接收账户是合同，则运行合同代码，直至完成或直至执行用完为止。
5. 如果由于发件人没有足够的资金而导致价值转移失败，或者代码执行耗尽，请还原除费用支付以外的所有状态更改，并将费用添加到矿工帐户。
6. 否则，将所有剩余天然气的费用退还给发送方，并将支付的天然气费用发送给矿工。

例如，假设合同的代码是：

    if !self.storage[calldataload(0)]:
        self.storage[calldataload(0)] = calldataload(32)

请注意，实际上，合同代码是用低级EVM代码编写的;,为了清晰起见，本示例是使用Serpent（我们的高级语言之一）编写的，可以编译为EVM代码。
假设合同的存储空开始，一个事务以10乙醚值，2000瓦斯值，0.001乙醚价格和64字节数据发送，字节0-31代表数字“2”，字节32-63代表,字符串`CHARLIE`。
在这种情况下状态转换函数的过程如下：

1. 检查交易是否有效并形成良好。
2. 检查交易发送者至少有2000 * 0.001 = 2以太。如果是，则从发件人的帐户中减去2以太。
3. 初始化气体= 2000;,假设交易长度为170个字节，字节费为5，则减去850，以便剩下1150个气体。
3. 从发件人账户中减去10个以上的乙醚，并将其添加到合同的账户。
4. 运行代码。在这种情况下，这很简单：它检查是否使用了索引为'2'的合约存储器，注意到它不是，因此它将索引'2'处的存储器设置为'CHARLIE'值。假设这需要187个气体，所以剩余气量是1150  -  187 = 963
5. 添加963 * 0.001 = 0.963乙醚返回发件人的帐户，并返回结果状态。

如果在交易接收端没有合同，那么总的交易费就等于所提供的“GASPRICE”乘以交易的长度（以字节为单位），而与交易一起发送的数据将是不相关的。

请注意，消息在回复方面与事务等效：如果消息执行耗尽，那么该消息的执行以及由该执行触发的所有其他执行将恢复，但是父执行不需要恢复。
这意味着合同另一份合同是“安全”的，就好像A用G气调用B一样，那么A的执行保证最多会损失G气。
最后，请注意，有一个操作码“CREATE”创建一个合同;,其执行机制通常与CALL类似，只是执行的输出决定了新创建的合同的代码。

## 代码执行

以太坊合同中的代码采用低级别，基于堆栈的字节码语言编写，称为“以太坊虚拟机代码”或“EVM代码”。
代码由一系列字节组成，其中每个字节代表一个操作。
一般来说，代码执行是一个无限循环，包括在当前程序计数器（从零开始）重复执行操作，然后将程序计数器递增1，直到代码结束或错误或“,检测到“STOP”或“RETURN”指令。
这些操作可以访问三种类型的存储数据的空间：

* **栈**，可以推送和弹出值的后进先出容器
* **内存**，一个无限扩展的字节数组
* 合同的长期**存储**，一个关键/价值商店。

与计算结束后重置的堆栈和内存不同，存储长期存在。

代码还可以访问传入消息的值，发送者和数据以及块头数据，代码也可以返回一个字节数组作为输出。

EVM代码的正式执行模型非常简单。
当以太坊虚拟机运行时，它的全部计算状态可以由元组（`block_state，transaction，message，code，memory，stack，pc，gas）`来定义，其中`block_state`是包含所有账户的全局状态，,包括余额和存储。
在每一轮执行开始时，当前的指令是通过'code'的'pc'字节找到的（如果pc> = len（code）`，则为0），每条指令都有自己的定义,它是如何影响元组的。
例如，`ADD`从堆栈中弹出两个物品，并推动他们的总和，使`gas`减少1，并且`pc`增加1，'SSTORE`弹出堆栈中的前两个物品，并将第二个物品插入,合同的存储在第一项指定的索引处。
尽管通过即时编译来优化以太坊虚拟机执行的方法有很多，但以太坊的基本实现可以在几百行代码中完成。

## 区块链和挖矿

![apply_block_diagram.png](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/apply_block_diagram.png)

以太坊区块链在很多方面与比特币区块链相似，不过它确实有一些区别。
以太坊和比特币在区块链架构方面的主要区别在于，与比特币不同，以太坊区块包含交易列表和最新状态的副本。
除此之外，块中还存储了另外两个值，即块号和难度。
以太坊的基本块验证算法如下:

1. 检查前面的块引用是否存在并且是有效的.
2. 检查块的时间戳是否大于参考的前一个块的时间戳，并在未来15分钟内
3. 检查区块编号，难度，交易根部，叔叔根部和气体限制（各种低级以太坊特定的概念）是否有效。
4. 检查块上的工作证明是否有效。
5. 让`S [0]`是前一个块末尾的状态。
6. 设`TX`为块的事务列表，其中`n`事务。对于`0 ... n-1`中的所有`i`，设置`S [i + 1] = APPLY（S [i]，TX [i]）`。如果任何应用程序返回一个错误，或者如果在这一点上阻塞的气体总量超过了`GASLIMIT`，则返回一个错误。
7. 让`S_FINAL`为`S [n]`，但是加上支付给矿工的块奖励。
8. 检查状态“S_FINAL”的Merkle树根是否等于块标题中提供的最终状态根。如果是的话，该块是有效的;,否则，这是无效的。

这种方法乍一看似乎效率非常低，因为它需要将每个区块的整个状态存储起来，但实际上效率应该与比特币相媲美。
原因是状态被存储在树结构中，并且在每个块之后只有一小部分树需要被改变。
因此，一般来说，在两个相邻块之间，绝大多数树应该是相同的，因此数据可以被存储一次并且使用指针（即子树的散列）被引用两次。
一种称为“Patricia树”的特殊树被用来实现这一点，包括对Merkle树概念的修改，允许节点被插入和删除，而不仅仅是有效地改变。
此外，由于所有的状态信息都是最后一个区块的一部分，因此不需要存储整个区块链历史 - 如果可以应用于比特币，则可以计算得出空间节省5-20倍的策略。

一个常见的问题是在物理硬件方面执行“where”合同代码。
这有一个简单的答案：执行合同代码的过程是状态转换函数的定义的一部分，这是块验证算法的一部分，所以如果一个事务被添加到块“B”中，则该事务产生的代码执行,将由现在和将来的所有节点执行，下载和验证块“B”。