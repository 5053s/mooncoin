macOS Build Instructions and Notes
====================================
The commands in this guide should be executed in a Terminal application.
The built-in one is located in `/Applications/Utilities/Terminal.app`.

Preparation
-----------
Install the macOS command line tools:

`xcode-select --install`

When the popup appears, click `Install`.

Then install [Homebrew](https://brew.sh).

Dependencies
----------------------

    brew install automake berkeley-db4 libtool boost miniupnpc openssl pkg-config protobuf python qt libevent qrencode

See [dependencies.md](dependencies.md) for a complete overview.

If you want to build the disk image with `make deploy` (.dmg / optional), you need RSVG

    brew install librsvg

Berkeley DB
-----------
It is recommended to use Berkeley DB 4.8. If you have to build it yourself,
you can use [the installation script included in contrib/](/contrib/install_db4.sh)
like so

```shell
./contrib/install_db4.sh .
```

from the root of the repository.

**Note**: You only need Berkeley DB if the wallet is enabled (see the section *Disable-Wallet mode* below).

Build Mooncoin Core
------------------------

1. Clone the Mooncoin Core source code and cd into `mooncoin`

        git clone https://github.com/mooncoin-project/mooncoin
        cd mooncoin

2.  Build Mooncoin Core:

    Configure and build the headless Mooncoin Core binaries as well as the GUI (if Qt is found).

    You can disable the GUI build by passing `--without-gui` to configure.

        ./autogen.sh
        ./configure
        make

3.  It is recommended to build and run the unit tests:

        make check

4.  You can also create a .dmg that contains the .app bundle (optional):

        make deploy

5.  Installation into user directories (optional):

        make install

    or

        cd ~/mooncoin/src
        cp mooncoind /usr/local/bin/
        cp mooncoin-cli /usr/local/bin/

Running
-------

Mooncoin Core is now available at `./src/mooncoind`

Before running, it's recommended that you create an RPC configuration file.

    echo -e "rpcuser=mooncoinrpc\nrpcpassword=$(xxd -l 16 -p /dev/urandom)" > "/Users/${USER}/Library/Application Support/Mooncoin/mooncoin.conf"

    chmod 600 "/Users/${USER}/Library/Application Support/Mooncoin/mooncoin.conf"

The first time you run mooncoind, it will start downloading the blockchain. This process could take several hours.

You can monitor the download process by looking at the debug.log file:

    tail -f $HOME/Library/Application\ Support/Mooncoin/debug.log

Other commands:
-------

    ./src/mooncoind -daemon # Starts the mooncoin daemon.
    ./src/mooncoin-cli --help # Outputs a list of command-line options.
    ./src/mooncoin-cli help # Outputs a list of RPC commands when the daemon is running.

Notes
-----

* Tested on OS X 10.10 Yosemite through macOS 10.13 High Sierra on 64-bit Intel processors only.

* Building with downloaded Qt binaries is not officially supported. See the notes in [#7714](https://github.com/bitcoin/bitcoin/issues/7714)
