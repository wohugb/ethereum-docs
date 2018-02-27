# cpp-ethereum - Ethereum C++ client

This repository contains [cpp-ethereum][1], the [Ethereum][2] C++ client.

It is the third most popular of the Ethereum clients, behind [geth][3] (the [go][4]
client) and [Parity][5] (the [rust][6] client).  The code is exceptionally
[portable][7] and has been used successfully on a very broad range
of operating systems and hardware.


## Contact

[![Gitter][8]][10]
[![GitHub Issues][9]](https://github.com/ethereum/cpp-ethereum/issues)

- Chat in [cpp-ethereum channel on Gitter][10].
- Report bugs, issues or feature requests using [GitHub issues][11].


## Getting Started

The Ethereum Documentation site hosts the **[cpp-ethereum homepage][1]**, which
has a Quick Start section.


Operating system | Status
---------------- | ----------
Ubuntu and macOS | [![TravisCI][12]](https://travis-ci.org/ethereum/cpp-ethereum)
Windows          | [![AppVeyor][13]](https://ci.appveyor.com/project/ethereum/cpp-ethereum)


## Building from source

### Get the source code

Git and GitHub is used to maintain the source code. Clone the repository by:

```shell
git clone --recursive https://github.com/ethereum/cpp-ethereum.git
cd cpp-ethereum
```

The `--recursive` option is important. It orders git to clone additional
submodules which are required to build the project.
If you missed it you can correct your mistake with command
`git submodule update --init`.

### Install CMake

CMake is used to control the build configuration of the project. Quite recent
version of CMake is required
(at the time of writing [3.4.3 is the minimum][14]).
We recommend installing CMake by downloading and unpacking the binary
distribution  of the latest version available on the
[**CMake download page**][15].

The CMake package available in your operating system can also be installed
and used if it meets the minimum version requirement.

> **Alternative method**
>
> The repository contains the
[scripts/install_cmake.sh][16] script that downloads
> a fixed version of CMake and unpacks it to the given directory prefix.
> Example usage: `scripts/install_cmake.sh --prefix /usr/local`.

### Install dependencies (Linux, macOS)

The following *libraries* are required to be installed in the system in their
development variant:

- leveldb

They usually can be installed using system-specific package manager.
Examples for some systems:

Operating system | Installation command
---------------- | --------------------
Debian-based     | `sudo apt-get install libleveldb-dev`
RedHat-based     | `dnf install leveldb-devel`
macOS            | `brew install leveldb`


We also support a "one-button" shell script
[scripts/install_deps.sh][17]
which attempts to aggregate dependencies installation instructions for Unix-like
operating systems. It identifies your distro and installs the external packages.
Supporting the script is non-trivial task so please [inform us][18]
if it does not work for your use-case.

### Install dependencies (Windows)

We provide prebuilt dependencies required to build the project. Download them
with the [scripts/install_deps.bat][19] script.

```shell
scripts/install_deps.bat
```

### Build

Configure the project build with the following command. It will create the
`build` directory with the configuration.

```shell
mkdir build; cd build  # Create a build directory.
cmake ..               # Configure the project.
cmake --build .        # Build all default targets.
```

On **Windows** Visual Studio 2015 is required. You should generate Visual Studio
solution file (.sln) for 64-bit architecture by adding
`-G "Visual Studio 14 2015 Win64"` argument to the CMake configure command.
After configuration is completed the `cpp-ethereum.sln` can be found in the
`build` directory.

```shell
cmake .. -G "Visual Studio 14 2015 Win64"
```

## Contributing

[![Contributors][20]][22]
[![Gitter][8]][10]
[![up-for-grabs][21]][23]

The current codebase is the work of many, many hands, with nearly 100
[individual contributors][22] over the course of its development.

Our day-to-day development chat happens on the
[cpp-ethereum][10] Gitter channel.

All contributions are welcome! We try to keep a list of tasks that are suitable
for newcomers under the tag
[up-for-grabs][23].
If you have any questions, please just ask.

Please read [CONTRIBUTING][24] and [CODING_STYLE][25]
thoroughly before making alterations to the code base.

All development goes in develop branch.


## Mining

This project is **not suitable for Ethereum mining**. The support for GPU mining
has been dropped some time ago including the ethminer tool. Use the ethminer tool from https://github.com/ethereum-mining/ethminer.

## Testing

To run the tests, make sure you clone https://github.com/ethereum/tests and point the environment variable
`ETHEREUM_TEST_PATH` to that path.

## Documentation

- [Internal documentation for developers][26].
- [Outdated documentation for end users][27].


## License

[![License][28]][30]

All contributions are made under the [GNU General Public License v3][29]. See [LICENSE][30].
[1]: http://cpp-ethereum.org
[2]: https://ethereum.org
[3]: https://github.com/ethereum/go-ethereum
[4]: https://golang.org
[5]: https://github.com/ethcore/parity
[6]: https://www.rust-lang.org/
[7]: http://cpp-ethereum.org/portability.html
[8]: https://img.shields.io/gitter/room/nwjs/nw.js.svg
[9]: https://img.shields.io/github/issues-raw/badges/shields.svg
[10]: https://gitter.im/ethereum/cpp-ethereum
[11]: issues/new
[12]: https://img.shields.io/travis/ethereum/cpp-ethereum/develop.svg
[13]: https://img.shields.io/appveyor/ci/ethereum/cpp-ethereum/develop.svg
[14]: CMakeLists.txt#L25
[15]: https://cmake.org/download/
[16]: scripts/install_cmake.sh
[17]: scripts/install_deps.sh
[18]: #contact
[19]: scripts/install_deps.bat
[20]: https://img.shields.io/github/contributors/ethereum/cpp-ethereum.svg
[21]: https://img.shields.io/github/issues-raw/ethereum/cpp-ethereum/up-for-grabs.svg
[22]: https://github.com/ethereum/cpp-ethereum/graphs/contributors
[23]: https://github.com/ethereum/cpp-ethereum/labels/up-for-grabs
[24]: CONTRIBUTING.md
[25]: CODING_STYLE.md
[26]: doc/index.rst
[27]: http://www.ethdocs.org/en/latest/ethereum-clients/cpp-ethereum/
[28]: https://img.shields.io/github/license/ethereum/cpp-ethereum.svg
[29]: https://www.gnu.org/licenses/gpl-3.0.en.html
[30]: LICENSE