# geth使用笔记

`geth` 的全称是 go-ethereum，是以太坊的 go 语言命令行客户端，也是最流行的客户端。

本文是对 `geth` 使用的简单介绍，以有一个感性的初步认识。 `geth` 完整教程请见另一篇文章以太坊文档

## 安装

安装教程请参见 [Building Ethereum](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum)

## 网络部署

以太坊除了主网络，还有各种各样的测试网络。使用 `geth` 前要先解决要进入哪一个网络。

## 主网络

### 同步区块

在主网络下，我们需要下载区块，以和网络保持同步。下载命令如下

快速同步模式： `geth --fast`，不会把所有区块数据下载到本地
全节点模式：geth，下载全部的区块数据，需要等待较长时间。

### 下载目录

主网络的区块数据的存放目录默认是 `/Users/<username>/Library/Ethereum`（Mac OS X 下）

* 其他系统下，可用方式找到默认存放路径：`geth -h`后搜索`--datadir`，后面紧跟的就是区块数据存放的目录
* 如果你想将区块下载到其他目录，可以使用命令`geth --fast --datadir "<path>"`（需要引号）

### 更优雅的同步方式

* `geth --fast console 2>network_sync.log`：同步时把同步日志文件重定向到`network_sync.log`中，同时切换到控制台模式（后面会提）。这样就可以边同步边使用命令。
* 用 `tail -f network_sync.log` 可以重新浏览到日志

不同客户端是可以共用区块数据的。如用 `geth` 同步的区块数据，可以在 `Ethereum Wallet` 或 `Mist` 等客户端使用（即使用其他客户端时，不必再次同步）

## 测试网络

测试网络是供开发和测试使用的。如只是单纯地使用钱包，可以跳过这一节。

测试网络需要从创世块开始挖起。部署方式如下

1. 新建创世块参数文件 `genesis.json`，内容如下
```json
{
    "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
    "difficulty": "10000",
    "gasLimit": "2100000",
    "alloc": {
        "7df9a875a174b3bc565e6424a0050ebc1b2d1d82": { "balance": "300000" },
        "f41c74c9ae680c1aa78f42e5647a62f353b7bdde": { "balance": "400000" }
    }
}
```
2. `geth --datadir ./ethdev init genesis.json`：初始化测试网络的以太坊。`./ethdev`是存放测试网络数据的文件夹，可以任意指定。
测试网络这样就部署好了，但里面一个区块和账户都没有。

参考：[Private network](https://github.com/ethereum/go-ethereum/wiki/Private-network)

## 使用

### 运行

运行客户端的方式如下

* 主网络
  * 如果区块数据在默认目录下：`geth`
  * 如果区块数据在其他目录下：`geth --datadir path/ethereum`
* 测试网络：`geth --datadir ./ethdev --networkid 15`。你只会连接与你的协议版本和 `networkid` 都相同的节点。主网络的 `networkid` 是 `1`，所以 `networkid` 只要不是 `1` 就可以

更常用的是运行客户端并进入控制台模式：`geth --datadir path/ethdev console 2>console.log`，然后使用另开窗口用 `tail -f console.log` 浏览日志。

## 控制台

一般操作都在控制台模式下进行。

* 进入控制台的方式为 `geth console`
* 要进入测试网络的控制台，同样加上参数 `--datadir ./ethdev`

常用操作如下

* 列出客户端中的所有账户：`eth.accounts`，这是一个列表。一般把 `u0 = eth.accounts[0]`，把账户赋给变量 `u0`，便于操作
* 新建账户：`personal.newAccount('123456')`：新建一个密码为 `123456` 的账户。注意，密码并不是密钥。
  * 账户数据保存在区块目录下的 `keystore` 文件夹中。建议把 `keystore` 文件夹复制一份到 U 盘里，备份起来。
* 查询账户 `u0` 的余额：`eth.getBalance(u0)`
* 获取最新区块号：`eth.blockNumber`
* 转账：`u0`转给`u1`
  * 需要先解锁 `u0`：`personal.unlockAccount(u0,'123456')`，第二个参数是密码
  * `eth.sendTransaction({from:u0, to:u1, value:web3.toWei(3, 'ether')})`，`u0`转给`u1`
* 挖矿
  * 开始挖矿：`miner.start()`，不停地挖矿，会返回一个`null`；`miner.start(1)`：只挖一个区块
    * 挖矿奖励默认发送给 `eth.accounts[0]`。
    * 测试网络里，把 `genesis.json` 中的 "`difficulty`" 设低一点，会比较好挖。
  * 停止挖矿：`miner.stop()`


作者：趁风卷

链接：https://www.jianshu.com/p/580235b9dd18

來源：简书

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。