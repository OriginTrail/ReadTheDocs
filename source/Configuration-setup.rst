..  _configuration-setup:

Node Configuration
==================

Pre-requirements
----------------

There's a minimum set of config parameters that need to be provided in order to run node, without
which the node will refuse to start.

- Ethereum Wallet: you must own a valid Ethereum wallet with at least 1000 TRAC tokens and at least 0.05 Ethers. See :ref:`wallet-setup` on how to obtain it.
- A public IP or open communication: It is required to have a public IP address, domain name or open network communication with internet.

Open Ports
----------

By default the node will use 8900, 5278 and 3000 ports. These can be mapped differently in configuration.
Make sure they’re not blocked by a firewall and are open to the public.
Please note: port 8900 is used for REST API access which is not available until OT node is fully started.
This can be concluded after following log message is displayed.

::

    info - OT Node started

Configuring the node
--------------------

Basic configuration
~~~~~~~~~~~~~~~~~~~~

To properly configure the node you will need to create a config file in JSON format and provide some
basic parameters for node operation. This file will be consumed by node upon start.
Let’s create the file **.origintrail_noderc** in OT node root dir and store all the information about
what kind of configuration we want to set up. The bare minimum of settings that needs to be provided
is two valid Ethereum wallet addresses:
- for the operational wallet (OW), which maps to node_wallet (OW public address) and node_private_key (OW private key)
- for the management wallet provide a public Ethereum address of your management wallet in the "management_wallet" parameter

You also need to provide a public address or domain name.

We create the **.origintrail_noderc** file with following content:

.. code:: json

    {
        "node_wallet": "your operational wallet address here",
        "node_private_key": "your operational wallet's private key here",
        "management_wallet": "your management wallet public key here",
        "network": {
            "hostname": "your external IP or domain name here",
            "remoteWhitelist": [ "IP or host of the machine that is requesting the import", "127.0.0.1"]
        },
        "blockchain": {
            "rpc_server_url": "url to your RPC server i.e. Infura or own Geth"
        }
    }

*node_wallet* and *node_private_key* - **operational wallet** Ethereum wallet address and its private key.

*management_wallet* - the **management wallet** for your node (note: the Management wallet private key is NOT stored on the node)

*hostname* - the public network address or hostname that will be used in P2P communication with other
nodes for node’s self identification.

*remoteWhitelist* - list of IPs or hosts of the machines ("host.domain.com") that are allowed to communicate with REST API.

*rpc_server_url* - an URL to RPC host server, usually Infura or own hosted Geth server. For more see :ref:`rpc-server-host`

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