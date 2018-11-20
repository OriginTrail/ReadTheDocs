..  _configuration-setup:

Node Configuration
======================================

.origintrail_noderc file configuration
-------------------------------------------------

Node running in production environment (in this context testnet), consumes configuration from ot-node/config/config.json.
Upon initializing, user has to setup wallet identity and few other key config parameters, and potentially override
default config parameters if needed (in example bellow arango database password).

In root project folder create file .origintrail_noderc

::

    touch .origintrail_noderc

with following content:

::

    {
        "node_wallet": "<Your wallet address here>",
        "node_private_key": "<Your wallet’s private key here>",
        "network": {
            "Hostname": "<Your external IP or domain name here>"
        },
        "database": {
            "password": "root"
        }
    }

where *node_wallet* and *node_private_key* are  Ethereum wallet address and its private key, and *hostname* is the public network address or hostname that will be used in P2P communication with other nodes for node’s self identification.

Finally, you can instruct docker container upon initialization to consume this file

::

$ sudo docker run -it --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v /abspath/to/your/.origintrail_noderc:/ot-node/.origintrail_noderc  origintrail/ot-node
   


Note: 
In case you are running your node manually (without docker), every time you change your configuration in .origintrail_noderc it's needed to run
this npm setup command to apply that configuration.

.. _here: https://github.com/OriginTrail/ot-node/blob/develop/.env.example
