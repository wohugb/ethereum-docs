# PyEthApp

[![Join the chat at https://gitter.im/ethereum/pyethapp][1]][1]

![image](https://img.shields.io/travis/ethereum/pyethapp.svg%0A%20%20%20%20%20:target:%20https://travis-ci.org/ethereum/pyethapp)

![image](https://coveralls.io/repos/ethereum/pyethapp/badge.svg%0A%20%20%20%20%20:target:%20https://coveralls.io/r/ethereum/pyethapp)

![image](https://img.shields.io/pypi/v/pyethapp.svg%0A%20%20%20%20%20:target:%20https://pypi.python.org/pypi/pyethapp)

![image](https://readthedocs.org/projects/pyethapp/badge/?version=latest%0A%20%20%20%20%20:target:%20https://readthedocs.org/projects/pyethapp/?badge=latest)

## 简介

pyethapp is the python based client implementing the [Ethereum][2] cryptoeconomic state machine.

Ethereum as a platform is focussed on enabling people to build new ideas using blockchain technology.

The python implementation aims to provide an easily hackable and extendable codebase.

pyethapp leverages two ethereum core components to implement the client:

- [pyethereum][3] - the core library, featuring the blockchain, the ethereum virtual machine,mining
- [pydevp2p][4] - the p2p networking library, featuring node discovery for and transport of multiple services over multiplexed and encrypted connections

## 安装

### 注意

Pyethapp runs on Python 2.7. If you don't know how to install an up-to-date version of Python, have a look
[here][5]. It is always advised to install system-dependecies with the help of a package manager (e.g. *homebrew* on Mac OS X or *apt-get* on Debian).

Please install a *virtualenv* environment for a comfortable Pyethapp installation. Also, it is always recommended to use it in combination with the [virtualenvwrapper][6] extension.

The [Homestead][7]-ready version of Pyethapp is `v1.2.0`.

### Ubuntu/Debian 上安装

First install the system-dependecies for a successful build of the Python packages:

```shell
$ apt-get install build-essential automake pkg-config libtool libffi-dev libgmp-dev
```

Installation of Pyethapp and it's dependent Python packages via [PyPI][8]:

```shell
($ mkvirtualenv pyethapp)
$ pip install pyethapp
```

### OS X 上安装

(More detailed instructions can be found in the [Mac OS X installation instructions][9])

First install the system-dependecies for a successful build of the Python packages:

```shell
$ brew install automake libtool pkg-config libffi gmp openssl
```

Installation of Pyethapp and it's dependent Python packages via [PyPI][8]:

```shell
($ mkvirtualenv pyethapp)
$ pip install pyethapp
```

### 开发版本

If you want to install the newest version of the client for development purposes, you have to clone it directly from GitHub.

First install the system dependencies according to your Operating System above, then:

```shell
($ mkvirtualenv pyethapp)
$ git clone https://github.com/ethereum/pyethapp
$ cd pyethapp
$ USE_PYETHEREUM_DEVELOP=1 python setup.py develop
```

This has the advantage that inside of Python's `lib/python2.7/site-packages` there is a direct link to your directory of Pyethapp's source code. Therefore, changes in the code will have immediate effect on the `pyethapp` command in your terminal.

## 连接网络

If you type in the terminal:

```shell
$ pyethapp
```

will show you all available commands and options of the client.

To get started, type:

```shell
($ workon pyethapp)
$ pyethapp account new
```

This creates a new account and generates the private key. The key-file is locked with the password that you entered and they are stored in the `/keystore` directory. You can't unlock the file without the password and there is no way to recover a lost one. Do **not delete the key-files**, if you still want to be able to access Ether and Contracts associated with that account.

To connect to the live Ethereum network, type:

```shell
($ workon pyethapp)
$ pyethapp run
```

This establishes the connection to Ethereum's p2p-network and downloads the whole blockchain on the first invocation.

For additional documentation how to use the client, have a look at the [Wiki][10].

### 数据目录

When running the client without specifying a data-directory, the blockchain-data and the keystore-folder will be saved in a default directory, depending on your Operating System.

on Mac OS X:

```shell
~/Library/Application\ Support/pyethapp
```

on Linux:

```shell
~/.config/pyethapp
```

This folder also holds the `config.yaml` file, in which you can modify your default configuration parameters.

To provide a different data-directory, e.g. for additionally syncing to the testnet, run the client with the `-d <dir>` / `--data-dir <dir>` argument.

## 可用网络

- Live (*Frontier* / *Homestead*)
- Test (*Morden*)

Currently there are two official networks available. The "Live Network" is called *Frontier* (soon to be *Homestead*) and this is what the client will connect to if you start it without any additional options.

Additionally there is the official test network called [Morden][11] which can be used to test new code or otherwise experiment without having to risk real money. Use the --profile command line option to select the test network:

```shell
$ pyethapp --profile testnet run
```

> **note**
>
> If you've previously connected to the live network you will also need
> to specify a new data directory by using the --data-dir option.

## 互动

You can interact with the client using the JSONRPC api or directly on the console.

- <https://github.com/ethereum/pyethapp/wiki/The_Console>
- <https://github.com/ethereum/pyethapp/blob/master/pyethapp/rpc_client.py>

## 状态

- Working PoC9 prototype
- interoperable with the go and cpp clients
- jsonrpc (mostly)
[1]: https://badges.gitter.im/Join%20Chat.svg
[2]: https://ethereum.org/
[3]: https://github.com/ethereum/pyethereum
[4]: https://github.com/ethereum/pydevp2p
[5]: https://wiki.python.org/moin/BeginnersGuide
[6]: https://virtualenvwrapper.readthedocs.org/en/latest/
[7]: https://ethereum-homestead.readthedocs.io/en/latest/introduction/the-homestead-release.html
[8]: https://pypi.python.org/pypi/pyethapp
[9]: https://github.com/ethereum/pyethapp/blob/develop/docs/installation_os_x.rst
[10]: https://github.com/ethereum/pyethapp/wiki
[11]: https://github.com/ethereum/wiki/wiki/Morden
[1]: https://gitter.im/ethereum/pyethapp?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge