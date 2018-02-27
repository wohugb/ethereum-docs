# Welcome to ethereumj

[![Slack Status][1]][1]
[![Gitter][2]][2]
[![Build Status][3]][3]
[![Coverage Status][4]][4]


# About
ethereumj is a pure-Java implementation of the Ethereum protocol. For high-level information about Ethereum and its goals, visit [ethereum.org][5]. The [ethereum white paper][6] provides a complete conceptual overview, and the [yellow paper][7] provides a formal definition of the protocol.

# Running EthereumJ

##### Adding as a dependency to your Maven project:

```
   <dependency>
     <groupId>org.ethereum</groupId>
     <artifactId>ethereumj-core</artifactId>
     <version>1.5.0-RELEASE</version>
   </dependency>
```

##### or your Gradle project:

```
   repositories {
       mavenCentral()
   }
   compile "org.ethereum:ethereumj-core:1.5.+"
```

As a starting point for your own project take a look at https://github.com/ether-camp/ethereumj.starter

##### Building an executable JAR
```
git clone https://github.com/ethereum/ethereumj
cd ethereumj
cp ethereumj-core/src/main/resources/ethereumj.conf ethereumj-core/src/main/resources/user.conf
vim ethereumj-core/src/main/resources/user.conf # adjust user.conf to your needs
./gradlew clean shadowJar
java -jar ethereumj-core/build/libs/ethereumj-core-*-all.jar
```

##### Running from command line:
```
> git clone https://github.com/ethereum/ethereumj
> cd ethereumj
> ./gradlew run [-PmainClass=<sample class>]
```

##### Optional samples to try:
```
./gradlew run -PmainClass=org.ethereum.samples.BasicSample
./gradlew run -PmainClass=org.ethereum.samples.FollowAccount
./gradlew run -PmainClass=org.ethereum.samples.PendingStateSample
./gradlew run -PmainClass=org.ethereum.samples.PriceFeedSample
./gradlew run -PmainClass=org.ethereum.samples.PrivateMinerSample
./gradlew run -PmainClass=org.ethereum.samples.TestNetSample
./gradlew run -PmainClass=org.ethereum.samples.TransactionBomb
```

##### Importing project to IntelliJ IDEA:
```
> git clone https://github.com/ethereum/ethereumj
> cd ethereumj
> gradlew build
```
  IDEA:
* File -> New -> Project from existing sources…
* Select ethereumj/build.gradle
* Dialog “Import Project from gradle”: press “OK”
* After building run either `org.ethereum.Start`, one of `org.ethereum.samples.*` or create your own main.

# Configuring EthereumJ

For reference on all existing options, their description and defaults you may refer to the default config `ethereumj.conf` (you may find it in either the library jar or in the source tree `ethereum-core/src/main/resources`)
To override needed options you may use one of the following ways:
* put your options to the `<working dir>/config/ethereumj.conf` file
* put `user.conf` to the root of your classpath (as a resource)
* put your options to any file and supply it via `-Dethereumj.conf.file=<your config>`
* programmatically by using `SystemProperties.CONFIG.override*()`
* programmatically using by overriding Spring `SystemProperties` bean

Note that don’t need to put all the options to your custom config, just those you want to override.

# Special thanks
YourKit for providing us with their nice profiler absolutely for free.

YourKit supports open source projects with its full-featured Java Profiler.
YourKit, LLC is the creator of [YourKit Java Profiler][8]
and [YourKit .NET Profiler][9],
innovative and intelligent tools for profiling Java and .NET applications.

![YourKit Logo](https://www.yourkit.com/images/yklogo.png)

# Contact
Chat with us via [Gitter][10]

# License
ethereumj is released under the [LGPL-V3 license][11].

[1]: http://harmony-slack-ether-camp.herokuapp.com/badge.svg
[2]: https://badges.gitter.im/Join%20Chat.svg
[3]: https://travis-ci.org/ethereum/ethereumj.svg?branch=master
[4]: https://coveralls.io/repos/ethereum/ethereumj/badge.png?branch=master
[5]: https://ethereum.org
[6]: https://github.com/ethereum/wiki/wiki/White-Paper
[7]: http://gavwood.com/Paper.pdf
[8]: https://www.yourkit.com/java/profiler/
[9]: https://www.yourkit.com/.net/profiler/
[10]: https://gitter.im/ethereum/ethereumj
[11]: LICENSE
[1]: http://ether.camp
[2]: https://gitter.im/ethereum/ethereumj?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge
[3]: https://travis-ci.org/ethereum/ethereumj
[4]: https://coveralls.io/r/ethereum/ethereumj?branch=master