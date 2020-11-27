Moneysocket Stablewallet Daemon
--------------------------

The Stablewallet Daemon (`stabled`) is an application that provides Moneysocket connections to accounts denominated in fiat currency. The daemon watches the exchange rate and varies the amount of satoshis available via the connection for settling transactions.

For paying invoices and receiving payment, it relies on its 'backend' being provided by a Terminus or other such Moneysocket provider that has access to that ability. The transactions are settled in satoshis, but the accounting for the accounts held by `stabled` is done in the account's native fiat currency at the time of settlement.


Disclaimer!
-----

Moneysocket is still new, under development and is Reckless with your money. Use this stuff at your own risk.


Dependencies
------------------------------------------------------------------------

This depends on [py-moneysocket](https://github.com/moneysocket/py-moneysocket)

`$ pip3 install https://github.com/moneysocket/py-moneysocket`

to install the library and libraries dependencies into your environment.

Additionally, this application has a few dependencies documented in [requirements.txt](requirements.txt). These can be installed to your environment via:

`$ pip3 install -r requirements.txt`

Running
------------------------------------------------------------------------

`stabled` needs a directory to persist the account data and to read its config from. By default this will be in `~/.stabled` with the config file being read from `~/stabled/stabled.conf`.

The configuration file controls the listening port being bound as well as the JSON-RPC settings for the CLI and other uses.

An example config file is provided [here](config/stabled.conf) which can be used as a basis.

`$ mkdir ~/.stabled`
`$ cp config/stabled.conf ~/.stabled`


To start `stabled`, run the executable:

`$ ./stabled`

CLI Interface
------------------------------------------------------------------------

The `stable-cli` executable can be called in a similar way to `bitcoin-cli`, `lightning-cli` and/or `lncli` executables which you are likely already familiar with.

The executable connects to the Terminus process via a JSON-RPC interface. By default, it will look at `~/.stabled/stabled.conf` for configuration details for making the connection.

To get a listing of available commands, the `help` command will print the usage:

`$ ./stable-cli help`

To get a summary of the current state, the `getinfo` command will produce a summary:

`$ ./stable-cli getinfo`


Connecting Assets
------------------------------------------------------------------------

For `stabled` to be able to send and receive via the fiat accounts that it tracks, it needs to be connected to operational Moneysocket connections on the backend it can use to forward those requests when appropriate.

The `connnectasset` command can be used to connect to a beacon to use as one such asset:

```
$ ./stable-cli connectasset moneysocket1lcqqzqdm8nlqqqgph5g8dhtkmzmq7upekrqvdv3kcfechlsqqyquzg87qqqsr0cpq8lqqqgpcvfsqzf3xgmjuvpwxqhrzqgpqqpq8lftry7vdcps
$ ./stable-cli getinfo
...
Assets:
        uuid: 345da37f-461a-45f9-bf47-a8833cbd9c3d
                wad: ₿ 10000.000 sat   payer: True   payee: True
...

```

To create an account stable in fiat the `createstable` account can be used.

```
$ ./stable-cli createstable 10 USD
$ ./stable-cli getinfo
...
Accounts:
        account-0: wad: $ 10.00 USD
```

To listen or connect, the `listen` and `connect` commands work the same as they do for accounts on the [terminus](https://github.com/moneysocket/terminus).

Likewise, the listening and outgoing connections can be cleared from an account with the `clear` command and the accounts can be removed entirely with the `rm` command.

The `creatpegged` and `rmpegged` commands can allow additional, non-fiat currency derivative asset to be defined. Once they are, `createstable` can be used to create an account stable in that asset's exchange rate.


Project Links
------------------------------------------------------------------------

- [Homepage](https://socket.money).
- [Twitter](https://twitter.com/moneysocket)
- [Telegram](https://t.me/moneysocket)
- [Donate](https://socket.money/#donate)
