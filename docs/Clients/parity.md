# [Parity][1] - fast, light, and robust Ethereum client

[![build status][2]][1]
[![codecov][3]][2]
[![Snap Status][4]][3]
[![GPLv3][5]][4]

- [Download the latest release here.][6]

### Join the chat!

Get in touch with us on Gitter:
[![Gitter: Parity][7]][15]
[![Gitter: Parity.js][8]][5]
[![Gitter: Parity/Miners][9]][6]
[![Gitter: Parity-PoA][10]][7]

Or join our community on Matrix:
[![Riot: +Parity][11]][16]

Be sure to check out [our wiki][12] and the [internal documentation][13] for more information.

----

## About Parity

Parity's goal is to be the fastest, lightest, and most secure Ethereum client. We are developing Parity using the sophisticated and cutting-edge Rust programming language. Parity is licensed under the GPLv3, and can be used for all your Ethereum needs.

Parity comes with a built-in wallet. To access [Parity Wallet][14] simply go to http://web3.site/ (if you don't have access to the internet, but still want to use the service, you can also use http://127.0.0.1:8180/). It includes various functionality allowing you to:

- create and manage your Ethereum accounts;
- manage your Ether and any Ethereum tokens;
- create and register your own tokens;
- and much more.

By default, Parity will also run a JSONRPC server on `127.0.0.1:8545` and a websockets server on `127.0.0.1:8546`. This is fully configurable and supports a number of APIs.

If you run into an issue while using Parity, feel free to file one in this repository or hop on our [Gitter][15] or [Riot][16] chat room to ask a question. We are glad to help!

**For security-critical issues**, please refer to the security policy outlined in [SECURITY.MD][17].

Parity's current release is 1.8. You can download it at https://github.com/paritytech/parity/releases or follow the instructions below to build from source.

----

## Build dependencies

**Parity requires Rust version 1.21.0 to build**

We recommend installing Rust through [rustup][18]. If you don't already have rustup, you can install it like this:

- Linux:
	```bash
	$ curl https://sh.rustup.rs -sSf | sh
	```

	Parity also requires `gcc`, `g++`, `libssl-dev`/`openssl`, `libudev-dev` and `pkg-config` packages to be installed.

- OSX:
	```bash
	$ curl https://sh.rustup.rs -sSf | sh
	```

	`clang` is required. It comes with Xcode command line tools or can be installed with homebrew.

- Windows
  Make sure you have Visual Studio 2015 with C++ support installed. Next, download and run the rustup installer from
	https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe, start "VS2015 x64 Native Tools Command Prompt", and use the following command to install and set up the msvc toolchain:
  ```bash
	$ rustup default stable-x86_64-pc-windows-msvc
  ```

Once you have rustup, install Parity or download and build from source

----

## Install from the snap store

In any of the [supported Linux distros][19]:

```bash
sudo snap install parity --edge
```

(Note that this is an experimental and unstable release, at the moment)

----

## Build from source

```bash
# download Parity code
$ git clone https://github.com/paritytech/parity
$ cd parity

# build in release mode
$ cargo build --release
```

This will produce an executable in the `./target/release` subdirectory.

Note: if cargo fails to parse manifest try:

```bash
$ ~/.cargo/bin/cargo build --release
```

Note: When compiling a crate and you receive the following error:

```
error: the crate is compiled with the panic strategy `abort` which is incompatible with this crate's strategy of `unwind`
```

Cleaning the repository will most likely solve the issue, try:

```bash
$ cargo clean
```

This will always compile the latest nightly builds. If you want to build stable or beta, do a `git checkout stable` or `git checkout beta` first.

----

## Simple one-line installer for Mac and Ubuntu

```bash
bash <(curl https://get.parity.io -Lk)
```

The one-line installer always defaults to the latest beta release.

## Start Parity

### Manually

To start Parity manually, just run

```bash
$ ./target/release/parity
```

and Parity will begin syncing the Ethereum blockchain.

### Using systemd service file

To start Parity as a regular user using systemd init:

1. Copy `./scripts/parity.service` to your
systemd user directory (usually `~/.config/systemd/user`).
2. To configure Parity, write a `/etc/parity/config.toml` config file, see [Configuring Parity][20] for details.

[1]: https://parity.io/
[2]: https://gitlab.parity.io/parity/parity/badges/master/build.svg
[3]: https://codecov.io/gh/paritytech/parity/branch/master/graph/badge.svg
[4]: https://build.snapcraft.io/badge/paritytech/parity.svg
[5]: https://img.shields.io/badge/license-GPL%20v3-green.svg
[6]: https://github.com/paritytech/parity/releases/latest
[7]: https://img.shields.io/badge/gitter-parity-4AB495.svg
[8]: https://img.shields.io/badge/gitter-parity.js-4AB495.svg
[9]: https://img.shields.io/badge/gitter-parity/miners-4AB495.svg
[10]: https://img.shields.io/badge/gitter-parity--poa-4AB495.svg
[11]: https://img.shields.io/badge/riot-%2Bparity%3Amatrix.parity.io-orange.svg
[12]: https://paritytech.github.io/wiki/
[13]: https://paritytech.github.io/parity/ethcore/index.html
[14]: http://web3.site/
[15]: https://gitter.im/paritytech/parity
[16]: https://riot.im/app/#/group/+parity:matrix.parity.io
[17]: SECURITY.md
[18]: https://www.rustup.rs/
[19]: https://snapcraft.io/docs/core/install
[20]: https://github.com/paritytech/parity/wiki/Configuring-Parity
[1]: https://gitlab.parity.io/parity/parity/commits/master
[2]: https://codecov.io/gh/paritytech/parity
[3]: https://build.snapcraft.io/user/paritytech/parity
[4]: https://www.gnu.org/licenses/gpl-3.0.en.html
[5]: https://gitter.im/paritytech/parity.js
[6]: https://gitter.im/paritytech/parity/miners
[7]: https://gitter.im/paritytech/parity-poa