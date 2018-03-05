# Go Ethereum (Geth)

以太坊协议的官方狗语言实现.

[![API Reference][1]][1]
[![Gitter][1]][1]

自动构建可用于稳定版本和不稳定版的主分支。二进制档案发布在[下载页][2].

## 构建源码

先决条件和详细的安装指导请阅读维基的[安装说明][3].

构建geth需要Go（1.7或更高版本）和C编译器。你可以使用你喜欢的包管理器来安装它们。一旦安装了依赖项，运行

```bash
  make geth
```

或者，建立一整套实用程序：

```bash
  make all
```

## 可执行文件

在`go-ethereum`项目中有几个在cmd目录下的封装/可执行文件。

| 命令       |         描述 |
|:----------:|-------------|
| **`geth`** | 我们主要的以太坊CLI客户端.它是以太坊网络的入口 (主网-, 测试网络- 或者私有入口), 能够作为一个完整的节点（默认）归档节点（保留所有历史状态）或轻节点运行（检索数据）.它可以作为其它进程的网关通过HTTP暴露的JSON RPC端点, WebSocket和/或IPC传输连接以太网络.`geth --help` 和 [CLI维基页面][4]查看命令选项.|
| `abigen` | 源代码生成器将以太坊合同定义转换为易于使用, 编译时类型安全的Go包.如果合同字节码也可用，它在普通[以太坊合同ABI][5]上运行，并具有扩展功能。但是它也接受Solidity源文件, 使发展更加精简.请看我们的[原生DApps][6]维基页面了解详情.|
| `bootnode` | 剥离了我们以太坊客户端实现的版本，只实现了网络节点发现协议, 但不运行任何更高级别的应用程序协议。 它可以用作轻量级引导程序节点来帮助找到专用网络中的对等设备.|
| `evm` | EVM开发工具版本（以太坊虚拟机），能够在可配置环境和执行模式下运行字节码片段。其目的是允许对EVM操作码进行隔离的，细粒度的调试 (例如 `evm --code 60ff60ff --debug`).|
| `gethrpctest` | 开发工具支持我们的[ethereum/rpc-test][7]测试套件，该套件验证了[Ethereum JSON RPC][8]规格的基线一致性。有关详细信息，请参阅[测试套件的自述文件][9]。|
| `rlpdump` | 开发者实用程序工具可将二进制RLP([递归长度前缀][10])转储(以太坊协议使用的数据编码既有网络也有共识)转换为用户更友好的分层表示 (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`).|
| `swarm`    | swarm守护进程和工具。这是swarm网络的入口点。`swarm --help`命令行选项和子命令。请参阅群集[文档](https://swarm-guide.readthedocs.io)。|
| `puppeth`    | 有助于创建新以太坊网络的CLI向导.|

## 运行 geth

查看所有可能的命令行标志超出了范围 (请咨询我们的[CLI Wiki页面][4]), 但是我们已经列举了几个常见的参数组合，让您快速了解如何运行自己的Geth实例。

### 以太坊网络上的完整节点

到目前为止，最常见的情况是人们希望简单地与以太坊网络互动：创建账户;,转移资金;,部署和与合同交互。
对于这种特殊的使用情况，用户不会关心过去几年的历史数据，所以我们可以快速同步到当前的网络状态。
要做到这一点：

```
$ geth --fast --cache=512 console
```

这个命令会:

 * 以快速同步模式启动geth（`--fast`）， 导致它下载更多的数据以避免处理以太坊网络的整个历史，这是非常CPU的密集型。
 * 将数据库的内存容量提高到512MB（`--cache = 512`），这可以极大地帮助同步时间，特别是对于HDD用户。此标志是可选的，您可以将其设置为高或低，但我们建议使用512MB  -  2GB范围。
 * 启动Geth的内置交互式[JavaScript控制台][11],(通过后面的`console`子命令) 通过它你可以调用所有的官方[`web3`方法][12]以及Geth自己的[管理API][13]。这也是可选的，如果你把它放在外面，你总是可以用`geth attach`附加到一个已经运行的Geth实例。

### 以太坊测试网络上的完整节点

向开发人员过渡，如果您想要创造以太坊合约，除非你掌握整个系统，否则你几乎肯定会这样做，而不需要真正的资金。
换一种说法， 而不是连接到主网络， 你想加入** test **网络与你的节点， 这完全等同于主要网络，但仅限于Play-Ether。

```
$ geth --testnet --fast --cache=512 console
```
 
`--fast`，`--cache`标志和`console`子命令具有与上述完全相同的含义，它们在测试网络上也同样有用。
如果您已跳到此处，请参阅上面的说明。

然而，指定`--testnet`标志会重新配置你的Geth实例：

* 而不是使用默认的数据目录（例如Linux上的`〜/ .ethereum`），Geth将把自己的层次更深一层地放到`testnet`子文件夹（Linux上的`〜/ .ethereum / testnet`）中。请注意，在OSX和Linux上，这也意味着连接到正在运行的testnet节点需要使用自定义端点，因为默认情况下`geth attach`将尝试附加到生产节点端点。例如`geth attach <datadir>/testnet/geth.ipc`.Windows用户不受此影响。
 * 客户端不会连接以太网主要网络，而是连接到测试网络，测试网络使用不同的P2P引导节点，不同的网络ID和生成状态。

* 注意：尽管有一些内部保护措施可以防止交易在主网络和测试网络之间交换，但您应该确保始终使用单独的帐户进行游戏币和真实资金。除非您手动移动账户，否则Geth将默认正确分离两个网络，并且不会在它们之间建立任何账户。*

### 配置

作为将众多标志传递给`geth`二进制文件的替代方法，您还可以通过以下方式传递配置文件：

```
$ geth --config /path/to/your_config.toml
```

要想知道文件应该如何看起来像你可以使用`dumpconfig`子命令来导出你现有的配置：

```
$ geth --your-favourite-flags dumpconfig
```

*注意：这只适用于geth v1.6.0及以上版本.*

### Docker快速入门

在你的机器上启动和运行以太坊的最快方法之一是使用Docker：

```
docker run -d --name ethereum-node -v /Users/alice/ethereum:/root \
           -p 8545:8545 -p 30303:30303 \
           ethereum/client-go --fast --cache=512
```

这将在快速同步模式下启动，数据库内存容量为512MB，就像上面的命令一样。
它还会在您的主目录中创建一个永久卷，以保存您的区块链以及映射默认端口。
还有一个“阿尔卑斯”标签可用于图像的纤细版本。

如果您想从其他容器和/或主机访问RPC，请不要忘记`--rpcaddr 0.0.0.0`。
默认情况下，`geth`绑定到本地接口，RPC端点不能从外部访问。

### 以编程方式连接Geth节点

作为一名开发人员，不久后你会想通过自己的程序开始与Geth和Ethereum网络进行交互，而不是通过控制台手动进行交互。
为了解决这个问题，Geth建立了对基于JSON-RPC的API（[标准API] [8]和[Geth特定API] [13]）的支持。
这些可以通过HTTP，WebSockets和IPC（基于unix的平台上的unix套接字，以及Windows上的命名管道）公开。

IPC接口默认启用并公开Geth支持的所有API，而HTTP和WS接口需要手动启用，并且由于安全原因仅公开一部分API。
这些可以打开/关闭，并按照您的预期进行配置。

基于HTTP的JSON-RPC API选项

  * `--rpc` 启用HTTP-RPC服务器
  * `--rpcaddr` HTTP-RPC服务器侦听接口（默认：“localhost”）
  * `--rpcport` HTTP-RPC服务器侦听端口（默认：8545）
  * `--rpcapi` 通过HTTP-RPC接口提供的API（默认：“eth，net，web3”）
  * `--rpccorsdomain` 以逗号分隔的接受跨源请求的域名列表（浏览器强制执行）
  * `--ws` 启用WS-RPC服务器
  * `--wsaddr` WS-RPC服务器侦听接口（默认值：“localhost”）
  * `--wsport` WS-RPC服务器侦听端口（默认值：8546）
  * `--wsapi` 通过WS-RPC接口提供的API（默认：“eth，net，web3”）
  * `--wsorigins` 接受websockets请求的起源
  * `--ipcdisable` 禁用IPC-RPC服务器
  * `--ipcapi` 通过IPC-RPC接口提供的API（默认值：“admin，debug，eth，miner，net，personal，shh，txpool，web3”）
  * `--ipcpath` 数据区中IPC套接字/管道的文件名（显式路径将其转义）

您需要使用您自己的编程环境的功能（库，工具等）通过HTTP，WS或IPC连接到配置了上述标志的Geth节点，并且您需要说[JSON-RPC] [14 ,]在所有运输。
您可以对多个请求重复使用相同的连接！

**注意：请理解在此之前打开基于HTTP / WS的传输所带来的安全隐患！,互联网上的黑客正在积极尝试用暴露的API来颠覆以太节点！
此外，所有浏览器标签都可以访问本地运行的Web服务器，因此恶意网页可能会尝试颠覆本地可用的API**

## 操作专用网络

维护您自己的专用网络更为重要，因为官方网络中许多认为理所当然的配置需要手动设置。

### 定义私人发起声明

首先，您需要创建您的网络的起源状态，所有节点都需要了解并达成一致。
这包含一个小的JSON文件（例如称为`genesis.json`）：

```json
{
  "config": {
        "chainId": 0,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

虽然我们建议将`nonce`改为某个随机值，以防止未知的远程节点能够连接到你，上述字段对于大多数目的应该没问题。
如果您想预先为某些帐户提供资金以便于测试，则可以使用帐户配置填充“alloc”字段：

```json
"alloc": {
  "0x0000000000000000000000000000000000000001": {"balance": "111111111"},
  "0x0000000000000000000000000000000000000002": {"balance": "222222222"}
}
```

你需要在启动之前初始化**每个** Geth节点，以确保所有区块链参数都正确设置：

```
$ geth init path/to/genesis.json
```

### 创建会合点

如果所有要运行的节点都初始化为所需的生成状态，则需要启动引导程序节点，其他人可以使用它来在网络中和/或通过互联网找到彼此。
干净的方法是配置和运行专用的引导节点：

```bash
$ bootnode --genkey=boot.key
$ bootnode --nodekey=boot.key
```

当bootnode在线时，它会显示一个[`enode` URL] [15]，其他节点可以用它来连接它并交换对等信息。
确保使用外部可访问的IP替换显示的IP地址信息（最可能是`[::]`）以获得实际的`enode` URL。

*注意: 您也可以使用完整的Geth节点作为启动节点, 但这是不太推荐的方法.*

### 启动你的成员节点

使用bootnode可以运行并且可以从外部访问（您可以尝试`telnet <ip> <port>`以确保它确实可以访问），请通过`--bootnodes`标志启动每个后续Geth节点，指向用于对等发现的bootnode。
可能还需要将私有网络的数据目录分开，所以还要指定一个自定义的“--datadir”标志。

```
$ geth --datadir=path/to/custom/data/folder --bootnodes=<bootnode-enode-url-from-above>
```

**注意：由于您的网络将与主网络和测试网络完全隔离，因此您还需要配置矿工来处理事务并为您创建新块。*

### 经营私人矿工

在公共以太坊网络上进行挖掘是一项复杂的任务，因为它只能使用GPU，需要OpenCL或CUDA启用的“ethminer”实例。
有关这种设置的信息，请查阅[EtherMining subreddit] [16]和[Genoil miner] [17]存储库。

对于实际应用来说已经足够了，因为它可以在不需要大量资源的情况下以正确的时间间隔产生稳定的数据块流（考虑在单线程上运行，不需要多个线程）。
要为挖掘启动Geth实例，请使用所有通常的标志运行它，并通过扩展：

```
$ geth <usual-flags> --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000
```

这将在单个CPU线程上开始挖掘块和事务，并将所有过程记录到`--etherbase`指定的帐户。
您可以通过将默认的天然气限制块更改为（`--targetgaslimit`），并在（`--gasprice`）接受价格交易来进一步调整采矿。

## 贡献

感谢您考虑帮助源代码 我们欢迎捐款,互联网上的任何人， 并感谢即使是最小的修复

如果你想为以太坊做出贡献， 请分叉，修复，提交并发送拉取请求,供维护人员查看和合并到主代码库中。
如果你想提交更多,尽管如此，请先核心开发者[我们的gitter频道][18]以确保这些变化符合项目的一般理念和/或得到一些,尽早的反馈，使您的努力更轻，以及我们的审查和合并,程序快捷简单。

请确保您的贡献符合我们的编码准则:

 * 代码必须遵守官方的Go [格式] [19]准则（即使用[gofmt] [20]）。
 * 代码必须遵循正式的Go [评注] [21]指导原则进行记录。
 * Pull请求需要基于`master`分支并打开。
 * 提交消息应该以其修改的软件包作为前缀。
   * 例如。 ,“eth，rpc：使跟踪配置可选”

请参阅[开发者指南][22]有关配置您的环境的更多详细信息, 管理项目依赖和测试程序.

## 证书

以太坊库（即`cmd`目录之外的所有代码）是在[GNU较宽松通用公共许可证v3.0][23], 也包含在我们的`COPYING.LESSER`文件中。

“去以太”二进制文件（即“cmd”目录内的所有代码）都是根据,[GNU通用公共许可证v3.0][24], 也包含在我们的`COPYING`文件中。

[1]: https://badges.gitter.im/Join%20Chat.svg
[2]: https://geth.ethereum.org/downloads/
[3]: https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum
[4]: https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options
[5]: https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI
[6]: https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go-bindings-to-Ethereum-contracts
[7]: https://github.com/ethereum/rpc-tests
[8]: https://github.com/ethereum/wiki/wiki/JSON-RPC
[9]: https://github.com/ethereum/rpc-tests/blob/master/README.md
[10]: https://github.com/ethereum/wiki/wiki/RLP
[11]: https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console
[12]: https://github.com/ethereum/wiki/wiki/JavaScript-API
[13]: https://github.com/ethereum/go-ethereum/wiki/Management-APIs
[14]: http://www.jsonrpc.org/specification
[15]: https://github.com/ethereum/wiki/wiki/enode-url-format
[16]: https://www.reddit.com/r/EtherMining/
[17]: https://github.com/Genoil/cpp-ethereum
[18]: https://gitter.im/ethereum/go-ethereum
[19]: https://golang.org/doc/effective_go.html#formatting
[20]: https://golang.org/cmd/gofmt/
[21]: https://golang.org/doc/effective_go.html#commentary
[22]: https://github.com/ethereum/go-ethereum/wiki/Developers'-Guide
[23]: https://www.gnu.org/licenses/lgpl-3.0.en.html
[24]: https://www.gnu.org/licenses/gpl-3.0.en.html
[1]: https://gitter.im/ethereum/go-ethereum?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge
[1]: https://camo.githubusercontent.com/915b7be44ada53c290eb157634330494ebe3e30a/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f676f6c616e672f6764646f3f7374617475732e737667
[1]: https://godoc.org/github.com/ethereum/go-ethereum