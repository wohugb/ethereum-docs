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
| **`geth`** | 我们主要的以太坊CLI客户端. 它是以太坊网络的入口 (主网-, 测试网络- 或者私有入口), 能够作为一个完整的节点（默认）归档节点（保留所有历史状态）或轻节点运行（检索数据）. 它可以作为其它进程的网关通过HTTP暴露的JSON RPC端点, WebSocket和/或IPC传输连接以太网络. `geth --help` 和 [CLI维基页面][4]查看命令选项. |
| `abigen` | 源代码生成器将以太坊合同定义转换为易于使用, 编译时类型安全的Go包. It operates on plain [Ethereum contract ABIs][5] with expanded functionality if the contract bytecode is also available. 但是它也接受Solidity源文件, 使发展更加精简. 请看我们的[原生DApps][6]维基页面了解详情. |
| `bootnode` | 剥离了我们以太坊客户端实现的版本，只实现了网络节点发现协议, 但不运行任何更高级别的应用程序协议。 它可以用作轻量级引导程序节点来帮助找到专用网络中的对等设备. |
| `evm` | Developer utility version of the EVM (Ethereum Virtual Machine) that is capable of running bytecode snippets within a configurable environment and execution mode. Its purpose is to allow isolated, fine-grained debugging of EVM opcodes (e.g. `evm --code 60ff60ff --debug`). |
| `gethrpctest` | Developer utility tool to support our [ethereum/rpc-test][7] test suite which validates baseline conformity to the [Ethereum JSON RPC][8] specs. Please see the [test suite's readme][9] for details. |
| `rlpdump` | Developer utility tool to convert binary RLP ([Recursive Length Prefix][10]) dumps (data encoding used by the Ethereum protocol both network as well as consensus wise) to user friendlier hierarchical representation (e.g. `rlpdump --hex CE0183FFFFFFC4C304050583616263`). |
| `swarm`    | swarm daemon and tools. This is the entrypoint for the swarm network. `swarm --help` for command line options and subcommands. See https://swarm-guide.readthedocs.io for swarm documentation. |
| `puppeth`    | a CLI wizard that aids in creating a new Ethereum network. |

## 运行 geth

Going through all the possible command line flags is out of scope here (please consult our
[CLI Wiki page][4]), but we've
enumerated a few common parameter combos to get you up to speed quickly on how you can run your
own Geth instance.

### 以太坊网络上的完整节点

By far the most common scenario is people wanting to simply interact with the Ethereum network:
create accounts; transfer funds; deploy and interact with contracts. For this particular use-case
the user doesn't care about years-old historical data, so we can fast-sync quickly to the current
state of the network. To do so:

```
$ geth --fast --cache=512 console
```

这个命令会:

 * Start geth in fast sync mode (`--fast`), causing it to download more data in exchange for avoiding
   processing the entire history of the Ethereum network, which is very CPU intensive.
 * Bump the memory allowance of the database to 512MB (`--cache=512`), which can help significantly in
   sync times especially for HDD users. This flag is optional and you can set it as high or as low as
   you'd like, though we'd recommend the 512MB - 2GB range.
 * Start up Geth's built-in interactive [JavaScript console][11],
   (via the trailing `console` subcommand) through which you can invoke all official [`web3` methods][12]
   as well as Geth's own [management APIs][13].
   This too is optional and if you leave it out you can always attach to an already running Geth instance
   with `geth attach`.

### 以太坊测试网络上的完整节点

Transitioning towards developers, if you'd like to play around with creating Ethereum contracts, you
almost certainly would like to do that without any real money involved until you get the hang of the
entire system. In other words, instead of attaching to the main network, you want to join the **test**
network with your node, which is fully equivalent to the main network, but with play-Ether only.

```
$ geth --testnet --fast --cache=512 console
```

The `--fast`, `--cache` flags and `console` subcommand have the exact same meaning as above and they
are equally useful on the testnet too. Please see above for their explanations if you've skipped to
here.

Specifying the `--testnet` flag however will reconfigure your Geth instance a bit:

 * Instead of using the default data directory (`~/.ethereum` on Linux for example), Geth will nest
   itself one level deeper into a `testnet` subfolder (`~/.ethereum/testnet` on Linux). Note, on OSX
   and Linux this also means that attaching to a running testnet node requires the use of a custom
   endpoint since `geth attach` will try to attach to a production node endpoint by default. E.g.
   `geth attach <datadir>/testnet/geth.ipc`. Windows users are not affected by this.
 * Instead of connecting the main Ethereum network, the client will connect to the test network,
   which uses different P2P bootnodes, different network IDs and genesis states.

*Note: Although there are some internal protective measures to prevent transactions from crossing
over between the main network and test network, you should make sure to always use separate accounts
for play-money and real-money. Unless you manually move accounts, Geth will by default correctly
separate the two networks and will not make any accounts available between them.*

### 配置

As an alternative to passing the numerous flags to the `geth` binary, you can also pass a configuration file via:

```
$ geth --config /path/to/your_config.toml
```

To get an idea how the file should look like you can use the `dumpconfig` subcommand to export your existing configuration:

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

This will start geth in fast sync mode with a DB memory allowance of 512MB just as the above command does.  It will also create a persistent volume in your home directory for saving your blockchain as well as map the default ports. There is also an `alpine` tag available for a slim version of the image.

Do not forget `--rpcaddr 0.0.0.0`, if you want to access RPC from other containers and/or hosts. By default, `geth` binds to the local interface and RPC endpoints is not accessible from the outside.

### 以编程方式连接Geth节点

As a developer, sooner rather than later you'll want to start interacting with Geth and the Ethereum
network via your own programs and not manually through the console. To aid this, Geth has built in
support for a JSON-RPC based APIs ([standard APIs][8] and
[Geth specific APIs][13]). These can be
exposed via HTTP, WebSockets and IPC (unix sockets on unix based platforms, and named pipes on Windows).

The IPC interface is enabled by default and exposes all the APIs supported by Geth, whereas the HTTP
and WS interfaces need to manually be enabled and only expose a subset of APIs due to security reasons.
These can be turned on/off and configured as you'd expect.

基于HTTP的JSON-RPC API选项

  * `--rpc` Enable the HTTP-RPC server
  * `--rpcaddr` HTTP-RPC server listening interface (default: "localhost")
  * `--rpcport` HTTP-RPC server listening port (default: 8545)
  * `--rpcapi` API's offered over the HTTP-RPC interface (default: "eth,net,web3")
  * `--rpccorsdomain` Comma separated list of domains from which to accept cross origin requests (browser enforced)
  * `--ws` Enable the WS-RPC server
  * `--wsaddr` WS-RPC server listening interface (default: "localhost")
  * `--wsport` WS-RPC server listening port (default: 8546)
  * `--wsapi` API's offered over the WS-RPC interface (default: "eth,net,web3")
  * `--wsorigins` Origins from which to accept websockets requests
  * `--ipcdisable` Disable the IPC-RPC server
  * `--ipcapi` API's offered over the IPC-RPC interface (default: "admin,debug,eth,miner,net,personal,shh,txpool,web3")
  * `--ipcpath` Filename for IPC socket/pipe within the datadir (explicit paths escape it)

You'll need to use your own programming environments' capabilities (libraries, tools, etc) to connect
via HTTP, WS or IPC to a Geth node configured with the above flags and you'll need to speak [JSON-RPC][14]
on all transports. You can reuse the same connection for multiple requests!

**Note: Please understand the security implications of opening up an HTTP/WS based transport before
doing so! Hackers on the internet are actively trying to subvert Ethereum nodes with exposed APIs!
Further, all browser tabs can access locally running webservers, so malicious webpages could try to
subvert locally available APIs!**

## 操作专用网络

Maintaining your own private network is more involved as a lot of configurations taken for granted in
the official networks need to be manually set up.

### 定义私人发起声明

First, you'll need to create the genesis state of your networks, which all nodes need to be aware of
and agree upon. This consists of a small JSON file (e.g. call it `genesis.json`):

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

The above fields should be fine for most purposes, although we'd recommend changing the `nonce` to
some random value so you prevent unknown remote nodes from being able to connect to you. If you'd
like to pre-fund some accounts for easier testing, you can populate the `alloc` field with account
configs:

```json
"alloc": {
  "0x0000000000000000000000000000000000000001": {"balance": "111111111"},
  "0x0000000000000000000000000000000000000002": {"balance": "222222222"}
}
```

With the genesis state defined in the above JSON file, you'll need to initialize **every** Geth node
with it prior to starting it up to ensure all blockchain parameters are correctly set:

```
$ geth init path/to/genesis.json
```

### 创建会合点

With all nodes that you want to run initialized to the desired genesis state, you'll need to start a
bootstrap node that others can use to find each other in your network and/or over the internet. The
clean way is to configure and run a dedicated bootnode:

```bash
$ bootnode --genkey=boot.key
$ bootnode --nodekey=boot.key
```

With the bootnode online, it will display an [`enode` URL][15]
that other nodes can use to connect to it and exchange peer information. Make sure to replace the
displayed IP address information (most probably `[::]`) with your externally accessible IP to get the
actual `enode` URL.

*注意: 您也可以使用完整的Geth节点作为启动节点, 但这是不太推荐的方法.*

### 启动你的成员节点

With the bootnode operational and externally reachable (you can try `telnet <ip> <port>` to ensure
it's indeed reachable), start every subsequent Geth node pointed to the bootnode for peer discovery
via the `--bootnodes` flag. It will probably also be desirable to keep the data directory of your
private network separated, so do also specify a custom `--datadir` flag.

```
$ geth --datadir=path/to/custom/data/folder --bootnodes=<bootnode-enode-url-from-above>
```

*Note: Since your network will be completely cut off from the main and test networks, you'll also
need to configure a miner to process transactions and create new blocks for you.*

### 经营私人矿工

Mining on the public Ethereum network is a complex task as it's only feasible using GPUs, requiring
an OpenCL or CUDA enabled `ethminer` instance. For information on such a setup, please consult the
[EtherMining subreddit][16] and the [Genoil miner][17]
repository.

In a private network setting however, a single CPU miner instance is more than enough for practical
purposes as it can produce a stable stream of blocks at the correct intervals without needing heavy
resources (consider running on a single thread, no need for multiple ones either). To start a Geth
instance for mining, run it with all your usual flags, extended by:

```
$ geth <usual-flags> --mine --minerthreads=1 --etherbase=0x0000000000000000000000000000000000000000
```

Which will start mining blocks and transactions on a single CPU thread, crediting all proceedings to
the account specified by `--etherbase`. You can further tune the mining by changing the default gas
limit blocks converge to (`--targetgaslimit`) and the price transactions are accepted at (`--gasprice`).

## 贡献

感谢您考虑帮助源代码 我们欢迎捐款,互联网上的任何人， 并感谢即使是最小的修复

如果你想为以太坊做出贡献， 请分叉，修复，提交并发送拉取请求,供维护人员查看和合并到主代码库中。
如果你想提交更多,尽管如此，请先核心开发者[我们的gitter频道][18]以确保这些变化符合项目的一般理念和/或得到一些,尽早的反馈，使您的努力更轻，以及我们的审查和合并,程序快捷简单。

请确保您的贡献符合我们的编码准则:

 * Code must adhere to the official Go [formatting][19] guidelines (i.e. uses [gofmt][20]).
 * Code must be documented adhering to the official Go [commentary][21] guidelines.
 * Pull requests need to be based on and opened against the `master` branch.
 * Commit messages should be prefixed with the package(s) they modify.
   * E.g. "eth, rpc: make trace configs optional"

请参阅[开发者指南][22]有关配置您的环境的更多详细信息, 管理项目依赖和测试程序.

## 证书

以太坊库（即`cmd`目录之外的所有代码）是在[GNU较宽松通用公共许可证v3.0][23], 也包含在我们的`COPYING.LESSER`文件中。

“去醚”二进制文件（即“cmd”目录内的所有代码）都是根据,[GNU通用公共许可证v3.0][24], 也包含在我们的`COPYING`文件中。
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