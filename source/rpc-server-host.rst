..  _rpc-server-host:

RPC server host
======================================

For an OT node to run it must communicate with the Ethereum blockchain. Such communication is achieved using
the Ethereum JSON `RPC`_ protocol and a RPC compatible server.

Starting from OT node version 2.0.50 it is required of users to provide their own RPC server to the node in order to run
it. Prior to this version all nodes used the same OriginTrail's provided URL hosted on Infura's server. Due to the changes
and plans in Infura service (see https://blog.infura.io/infura-dashboard-transition-update-c670945a922a ) we are going
forward with the proposed changes and encourage users to register their own Infura API key or host their own Ethereum
node both which can be used as an RPC host.


RPC server configuration
~~~~~~~~~~~~~~~~~~~~~~~~

The RPC server URL must be provided in the OT node's configuration file and it should be placed in the *blockchain* section
as *rpc_server_url*. For example:

.. code:: json

        "blockchain": {
            "rpc_server_url": "https://my.rpc.server.url:9000/"
        }

For more on how to set configuration file go to :ref:`Configuration-setup`

Using Infura as RPC host
~~~~~~~~~~~~~~~~~~~~~~~~

Using Infura gives a lot of advantages such as not needing to host own server or configuring the Ethereum node client
or even not scaling the whole infrastructure.

In order to use it create an account at https://infura.io . Once logged-in you can create a project for which you'll
have project ID, project secret and the endpoint. That endpoint is the RPC server URL needed for the node to run. Make
sure you pick the right one for the target network. Select *RINKEBY* to get URL that will be used in the Testnet or
*MAINNET* for the OriginTrail's mainnet.


Using own Ethereum node as RPC host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use the Ethereum node as an RPC server make sure it is properly configured and RPC feature is enabled (*--rpc* parametar).
For more details on how to install and configure Ethereum node see: https://github.com/ethereum/go-ethereum/wiki/Installing-Geth .

Once the Ethereum node is up and running use its URL to point the OT node to use it.

.. _RPC: https://github.com/ethereum/wiki/wiki/JSON-RPC
