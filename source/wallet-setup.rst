..  _wallet-setup:

Identity Configuration
=======================

Each node in the OriginTrail network is assigned an identity that represents it on the network.
For the nodes to be able to interact via the OriginTrail protocol, they need to poses TRAC tokens,
as well as Ether assigned to their identity. TRAC tokens are tokens based on the ERC20 token standard,
and can be handled by any ERC20 supporting Ethereum wallet.

To setup a node and it's identity on the OriginTrail mainnet, you will need TRAC and ETH.
In case you want to test out the node and setup process, we suggest you do it on OriginTrail Testnet
(using Ethereum Rinkeby network) which operates with Rinkeby ETH and tokens, which have no commercial value.
If you need test TRAC token please visit our faucet page.

OriginTrail is not responsible for setup and use of your wallets, keys and related software.
Our team does not and cannot provide support for wallet setup and management.
We strongly advise anyone intending to run a node on ODN to get familiar with the principles and technology behind cryptocurrency
wallets in order to be informed about its operations, safety and usage. With improper use or safety precautions your
funds on the wallets can be lost!

Node network profile
~~~~~~~~~~~~~~~~~~~~~

Each node on the OriginTrail network is represented by their profile, which contains information about node identification,
profile and token balance. The node profile is a smart contract that consists of the nodes ERC725 identity,
network identity and other operational information. This contract is also responsible for operating with tokens.

ERC725 Identity
~~~~~~~~~~~~~~~~

ERC 725 is a proposed standard for blockchain-based identity authored by Fabian Vogelsteller,
creator of the ERC 20 token standard and Web3.js. ERC 725 describes proxy smart contracts that can be controlled by multiple
keys and other smart contracts and it's contracts are deployed on Ethereum blockchain.

The OriginTrail node identity is compatible with the ERC725 standard and utilizes it for key management.
It distinguishes two different types of keys in the identity contract:

- the operational key (wallet), whose private key is stored on the node itself and is used to perform a multitude of operations in the ODN (signing, execution, etc). It requires a small balance of ETH in order to be able to publish transactions to the blockchain, and it can be filled periodically. No TRAC tokens are required for this wallet
- the management key (wallet), whose private key is NOT stored on the node and is used to deal with the funds (TRAC rewards) and to manage the keys associated with the ERC725 identity. The management wallet can be any ERC20 supporting wallet (Trezor, Ledger, Metamask etc).

This approach is taken as a convenience measure to provide for flexibility with key management and to minimize the risk of loosing funds in case of the operational key stored on the node somehow gets compromised. It is the node holders responsibility to keep both their node and wallet safe.

**Note:** *This system is supported from version v2.0.38. Previously the OT node supported only one wallet which had the role of both the operational wallet and the management wallet. If you have installed a mainnet node before version 2.0.38, after the update your node identity will have the same key assigned to both your operational and management wallet. In order to change the your management wallet to the operational wallet you will need to execute the key management functions on your identity smart contract.*

The exact steps are:

1. add a new management key with your old operational wallet by calling the addKey function in your ERC725 identity contract, with purposes [1,2,3,4].
2. remove the operational wallet from the ERC725 identity contract by calling the removeKey function with your either your new management key or old operational wallet (the one being removed)
3. add a new operational key to the ERC725 contract by calling addKey with purposes [2,4]
4. Once this is complete, stop your node and open the config file and input the management wallet address in the field “**management_wallet**”. Please make sure you enter the correct wallet address
5. Once all these steps are complete, restart your node. Once the node starts it will check the management_wallets permission and if valid, the update of the node will be complete.

To easily perform the operations above, a convenient UI is available at this `link`_.

If you want to execute the smart contract functions manually, here's a `tutorial`_ on how to call a smart contract function through MyEtherWallet.

How do I obtain my current ERC725 identity?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have installed a node, the easiest way to find value of your ERC725 identity at this moment is to look for it in the node’s log.

.. code:: bash

    notify - Identity created for node ab2e1b1e520cac0d1321cd3760c2e7473970ec8a.

    Identity is 0x99c67054a8c7b7fa62243f0446eacd80c6ff0aff.


The last value (in above case **0x99c67054a8c7b7fa62243f0446eacd80c6ff0aff**) represents the ERC725 identity.
Alternatively, you can copy it from node’s container

.. code:: bash

    # Copies file to HOME dir

    docker cp otnode:/ot-node/data/erc725_identity.json ~

one of the files here should be erc725_identity.json, whose value should exactly match value shown in the log line.

For manual installation file can be found at **~/.origintrail_noderc/producion** for testnet or **~/.origintrail_noderc/mariner** for mainnet.

Network identity
~~~~~~~~~~~~~~~~~

Apart from the ERC725 identity the node posesses the network identity as well.
To find out your nodes network identity, simply find a log line similar to this:

.. code:: bash

    notify - My network identity: ab2e1b1e520cac0d1321cd3760c2e7473970ec8a

and this value ( in above example **ab2e1b1e520cac0d1321cd3760c2e7473970ec8a**) it what you are looking for.
Alternatively, you can copy it from node’s container



.. code:: bash

    # Copies file to HOME dir

    docker cp otnode:/ot-node/data/identity.json ~

For manual installation file can be found at **~/.origintrail_noderc/producion** for testnet or **~/.origintrail_noderc/mariner** for mainnet.

Some users might notice that in data folder there is also a file nameed identity.json,
and that value stored in this file is different from the nodes identity value from logs.
Identity.json contains atomic information about the node identity - the identity itself is created based on the contents of the file.

**Important note:** *If you wish to run an identical node on another machine, then in addition to backing up you node operational private key, you should back up erc725_identity.json and identity.json files. There will be a separate article on how to start node with previously backed up identities. For now, be aware if you start a node on a different machine with providing only the operational private key, the node will create completely new identities, and you will end up having different node on the network.*

Setting up a node with predefined identities
Let’s say user already have network identity file and ERC725 identity file in home dir.

Let's say user already have network identity file and ERC725 identity file in home dir.

- .origintrail_noderc - node configuration.
- .identity.json - network identity.
- .erc725_identity.json - ERC 725 idenity.

::

        docker run -it --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000
        -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc
        -v ~/.identity.json:/ot-node/data/identity.json
        -v ~/.erc725_identity.json:/ot-node/data/erc725_identity.json
        quay.io/origintrail/otnode-mariner:release_mariner

Please note this example is for mainnet.
For testnet use **origintrail/ot-node** instead **quay.io/origintrail/otnode-mariner:release_mariner**



What about tokens and how do I get them on my wallet?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The tokens staked and locked for agreements your node is performing on the network are stored on a smart contract (not your wallet) which is part of the OriginTrail protocol.
You can withdraw these tokens once the agreement that the tokens were used to reimburse your node for has been
fulfilled (the agreement time has elapsed and your node has kept the data for that required time).

The token withdrawal process is a two step procedure. To withdraw the tokens from the Profile smart contract to your management node wallet,
you need to perform two function calls:

- **startTokenWithdrawal**, to initiate the withdrawal process by providing your ERC725 identity address and the amount you want to withdraw

- **withdrawTokens**, to complete the withdrawal process by providing your ERC725 identity.

- **Note:** *both function call transactions need to be executed with your ERC725 management wallet, otherwise they will fail.*

This two step process ensures that your node gracefully takes care of the token withdrawal on its network profile by properly adapting in the withdrawal period to responding to new agreement offers.
The withdrawal period is currently set to 5 minutes.

**To make it easier to interact with the node ERC725 identity, to deposit and withdraw tokens, we have provided a convenient UI at this** `link`_.

If you want to execute the smart contract functions manually, here's a `tutorial`_ on how to call a smart contract function through MyEtherWallet.


.. _link: https://node-profile.origintrail.io/
.. _tutorial: https://knowledge-base.origintrail.io/identity-configuration/how-to-manually-call-a-smart-contract-function-through-myetherwallet-example-of-token-withdrawal
.. _Instructions: https://knowledge-base.origintrail.io/
.. _here: http://github.com/OriginTrail/ot-yimishiji-pilot/wiki/Usage
.. _video: https://youtu.be/1UaB8OG_lgw
.. _metamask.io: https://metamask.io/
.. _faucet: http://www.origintrail.io/faucet 
