..  _configuration-setup:

Node Configuration
==================

Pre-requirements
----------------

There's a minimum set of config parameters that needs to be provided in order to run node without
which node will refuse to start.

- Ethereum Wallet: you must own a valid Ethereum wallet with at least 100 test TRAC tokens and at least 0.01 Ethers. See :ref:`wallet-setup` on how to obtain it.
- Public IP or open communication: It is required to have a public IP address, domain name or open network communication with internet.

Open Ports
----------

By default node will use 8900, 5278 and 3000 ports. These can be mapped differently in configuration.
Make sure they’re not blocked by firewall and open to the public.
Please note: port 8900 is used for REST API access which is not available until OT node is fully started.
This can be concluded after following log message is displayed.

::

    info - OT Node started

Configuring the node
--------------------

Basic configuration
~~~~~~~~~~~~~~~~~~~~

To properly configure the node you will need to create a config file in JSON format and provide some
basic information about how the node should work. This file will be consumed by node upon start.
Let’s create the file **.origintrail_noderc** in OT node root dir and store all the information about
what kind of configuration we want to set up. The bare minimum of settings that needs to be provided
is a valid Ethereum wallet and public address or domain name.
We create the **.origintrail_noderc** file with following content:

.. code:: json

    {
        "node_wallet": "your wallet address here",
        "node_private_key": "your wallet's private key here",
        "network": {
            "hostname": "your external IP or domain name here",
            "remoteWhitelist": [ "IP or host of the machine that is requesting the import", "127.0.0.1"]
        }
    }

*node_wallet* and *node_private_key* - Ethereum wallet address and its private key.

*hostname* - the public network address or hostname that will be used in P2P communication with other
nodes for node’s self identification.

*remoteWhitelist* - list of IPs or hosts of the machines ("host.domain.com") that are allowed to communicate with REST API.


Deposit on demand
~~~~~~~~~~~~~~~~~~~~

With the config option “**deposit_on_demand**” set to “**true**” your node will generate transactions for token deposit every time it doesn’t have enough funds for the job staked on your profile.
The tokens will be pulled from your wallet address automatically so your node is able to apply for the job.
As there are many jobs the node will generate many transactions for token deposits, most of the time a very small amounts of TRAC.
This procedure can spend larger amounts of ETH in order to pay GAS for the transactions.

By setting “**deposit_on_demand**” option to “**false**”, your node will not automatically deposit the tokens required for the jobs but instead will not apply for the job for which it does not have enough funds.
You will have to make sure your node has enough TRAC by manually depositing the TRAC tokens to your profile.
We recommend that you deposit larger amounts of TRAC so the number of depositing transactions is as small as possible.

- In order to disable the **deposit_on_deman** option please edit your **config.json** file, which can be found in your "**/ot-node/config/**" directory, by setting it to "**false**" as described below.

**DISABLING** the **deposit_on_demand** option:

.. code:: json

    {
        "node_wallet": "your wallet address here",
        "node_private_key": "your wallet's private key here",
        "network": {
            "hostname": "your external IP or domain name here",
            "remoteWhitelist": [ "IP or host of the machine that is requesting the import", "127.0.0.1"],
            "deposit_on_demand": false
        }
    }


**ENABLING** the **deposit_on_demand** option:

- In order to enable the **deposit_on_deman** option please edit your **config.json** file, which can be found in your "**/ot-node/config/**" directory, by setting it to "**true**" as described below:

.. code:: json

    {
        "node_wallet": "your wallet address here",
        "node_private_key": "your wallet's private key here",
        "network": {
            "hostname": "your external IP or domain name here",
            "remoteWhitelist": [ "IP or host of the machine that is requesting the import", "127.0.0.1"],
            "deposit_on_demand": false
        }
    }


Configuration file
~~~~~~~~~~~~~~~~~~

In general OT node uses [RC](https://www.npmjs.com/package/rc) nodejs package to load configuration and
everything mentioned there applies to the OT node.

Application name that will be used in detecting the config files is **origintrail_node**. Translated from
RC package page a configuration file lookup will be like this (from bottom towards top):

+ command line arguments, parsed by minimist (e.g. --foo baz, also nested: --foo.bar=baz)
+ environment variables prefixed with *origintrail_node_*
+ or use "__" to indicate nested properties (e.g. origintrail_node_foo__bar__baz => foo.bar.baz)
+ if you passed an option --config file then from that file
+ a local *.origintrail_noderc* or the first found looking in ./ ../ ../../ ../../../ etc.
+ $HOME/.origintrail_noderc
+ $HOME/.origintrail_node/config
+ $HOME/.config/origintrail_node
+ $HOME/.config/origintrail_node/config
+ /etc/origintrail_noderc
+ /etc/origintrail_node/config
+ the defaults object you passed in.

All configuration sources that were found will be flattened into one object, so that sources earlier in
this list override later ones.
