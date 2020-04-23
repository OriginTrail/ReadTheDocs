..  _identity-management:

Identity management
===================

Introduction
------------

On the OriginTrail Decentralized Network, your node is identified using two identities, a network layer identity, and a blockchain layer identity.

Network Identity
~~~~~~~~~~~~~~~~

The network layer identity is used for other nodes on the network to contact your node, whether it be for offers, queries, or any other functionalities

Blockchain profile and identity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Each node on the OriginTrail network is represented by their profile, which contains information about node identification, profile and token balance.

The node profile is a structure inside of a smart contract that is identified by your node's ERC725 identity, and contains network identity and other operational information. This contract is also responsible for operating with tokens.

ERC 725 is a proposed standard for blockchain-based identity authored by Fabian Vogelsteller, creator of the ERC 20 token standard and Web3.js. ERC 725 describes proxy smart contracts that can be controlled by multiple keys and other smart contracts and it’s contracts are deployed on Ethereum blockchain.

The OriginTrail Blockchain Identity is an ERC725 compatible smart contract and utilizes the standard for key management. It distinguishes two different types of keys in the identity contract:

-  The operational key (wallet), whose private key is stored on the node itself and is used to perform a multitude of operations in the ODN (signing, execution, etc). It requires a small balance of ETH in order to be able to publish transactions to the blockchain, and it can be filled periodically. No TRAC tokens are required for this wallet, except for initial node startup

-  The management key (wallet), whose private key is NOT stored on the node and is used to deal with the funds (TRAC rewards) and to manage the keys associated with the ERC725 identity. The management wallet can be any ERC20 supporting wallet (Trezor, Ledger, MetaMask etc).

This approach is taken as a safety and convenience measure to provide flexibility with key management and to minimize the risk of losing funds in case the operational key stored on the node somehow gets compromised. It is the node holder's responsibility to keep both their node and wallet safe.

Identity values and identity files
----------------------------------

ERC725 Identity value
~~~~~~~~~~~~~~~~~~~~~

On an installed node the easiest way to find the ERC725 identity value is to look for it in the node log

    ``notify - Identity created for node ab2e1b1e520cac0d1321cd3760c2e7473970ec8a.``
    ``Identity is 0x99c67054a8c7b7fa62243f0446eacd80c6ff0aff.``

The last value (in above case 0x99c67054a8c7b7fa62243f0446eacd80c6ff0aff) represents the blockchain identity. Alternatively, it can be copied from node’s container


.. code:: bash

    # Copies file to HOME dir
    docker cp otnode:/ot-node/data/erc725_identity.json ~

Network Identity value
~~~~~~~~~~~~~~~~~~~~~~

In order to find out the node's network identity, it can be found in the node startup log, looking similar to this:

    ``notify - My network identity: ab2e1b1e520cac0d1321cd3760c2e7473970ec8a``

and this value ( in above example ab2e1b1e520cac0d1321cd3760c2e7473970ec8a) is the value of the network identity. Alternatively, it can be copied from the node’s container

.. code:: bash

    # Copies file to HOME dir
    docker cp otnode:/ot-node/data/identity.json ~

Both the network and blockchain identities will be automatically generated when a node is started for the first time (if they were not pre generated), creating identity files in the configuration directory. These files enable the node to have the same identity every subsequent time it is ran.

We highly recommend backing up the identity files as soon as the node is set up.

How to use existing identity files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you wish to run an identical node on another machine, then in addition to backing up you node configuration (.origintrail\_noderc) file, you should back up erc725\_identity.json and identity.json files. If you start a node on a different machine without providing the identity files, the node will create completely new identities, and you will end up having a different node on the network.

Let’s say a user already has the network and ERC725 identity files in the home dir.

-  ``.origintrail_noderc`` - node configuration

-  ``.identity.json`` - network identity

-  ``.erc725_identity.json`` - ERC725 identity

.. code:: bash

    docker run -it --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc -v ~/.identity.json:/ot-node/data/identity.json -v ~/.erc725_identity.json:/ot-node/data/erc725_identity.json quay.io/origintrail/otnode:release_mainnet

Please note this example is for mainnet. For testnet use ``origintrail/otnode:release_testnet`` instead \ `quay.io/origintrail/otnode:release\_mainnet <http://quay.io/origintrail/otnode:release_mainnet>`__

Identity management
-------------------

To make it easier to interact with your node blockchain profile (to deposit and withdraw tokens) and identity (to edit your operational or management keys), we have provided a convenient UI at `this link <https://node-profile.origintrail.io/>`__\ .

Token management
~~~~~~~~~~~~~~~~

Staking and locking tokens
^^^^^^^^^^^^^^^^^^^^^^^^^^

In order for a node to create or accept offers on the network, it needs to stake tokens. Those tokens are locked for the duration of the offer and cannot be directly withdrawn by either party. A data holder can pay out a portion of the tokens allotted for the offer, proportional to the percentage of the agreed holding time. Paying out the tokens includes transferring tokens from the data creator's profile to the data holder's and unlocking the data holder's staked tokens.

Withdrawing tokens
^^^^^^^^^^^^^^^^^^

Tokens which are not locked can be withdrawn to your management wallet. Be aware that withdrawal is a two step process, where the node requests to withdraw tokens and, after the withdrawal period, the tokens are transferred to the management wallet which executed the second step. This two step process ensures that your node gracefully adapts to new offers within the withdrawal period. The withdrawal period is currently set to 5 minutes.

You can stake or withdraw tokens on the `Node Profile interface <https://node-profile.origintrail.io/>`__\ .

Key (wallet) management
~~~~~~~~~~~~~~~~~~~~~~~

Important: Please note that changing a wallet in the node configuration file does not change the wallet in your ERC725 identity. The wallet you wish to add first needs to have the appropriate permissions on the ERC725 identity before it can be changed in the node configuration.

Multiple management and operational wallets can be registered on a single ERC725 identity. One management wallet must always be registered. It is possible to remove all operational wallets and use a management wallet as the operational wallet at the same time, but we strongly discourage this scenario as it is not as secure as using separate wallets.

We recommend using the Node Profile interface for any changes of key permissions on the ERC725 identity.

Changing operational keys (wallets)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Changing the operational wallet on a node is done using the following steps

#. Add new operational wallet to the ERC725 identity

#. Set the new operational wallet and corresponding private key as the node\_wallet and node\_private\_key in the node configuration file (.origintrail\_noderc)

#. Restart the OriginTrail node

#. Remove the old operational wallet from the ERC725 identity

Changing management keys (wallets)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Changing the management wallet is done by adding the new management wallet to the ERC725 identity and then removing the old one.

The latest version of OriginTrail node supports data backup and restoration. With it you can save all your current data and restore it on a clean docker image. Below is a guide on how to back up and restore your node.

If you need additional assistance there is a support chat available on our knowledge base.


.. _link: https://node-profile.origintrail.io/
.. _tutorial: https://knowledge-base.origintrail.io/identity-configuration/how-to-manually-call-a-smart-contract-function-through-myetherwallet-example-of-token-withdrawal
.. _Instructions: https://knowledge-base.origintrail.io/
.. _here: http://github.com/OriginTrail/ot-yimishiji-pilot/wiki/Usage
.. _video: https://youtu.be/1UaB8OG_lgw
.. _metamask.io: https://metamask.io/
.. _faucet: http://www.origintrail.io/faucet