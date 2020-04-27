Getting started
===============

Setup & manage your node
------------------------

Hardware requirements
~~~~~~~~~~~~~~~~~~~~~

The recommended minimum specifications are 1CPU, and 2GB RAM with at least 10GB of storage space.

Installation instructions
~~~~~~~~~~~~~~~~~~~~~~~~~

Read Me First
+++++++++++++++
Please keep in mind that we will give our best to support you while setting up and testing the nodes. Some features are subject to change and we are aware that some defects may show up on different installations and usage scenarios.

If you need help installing OT Node or troubleshooting your installation, you can either:

-  engage in our Discord community and post your question,

-  contact us directly via email at \ `tech@origin-trail.com <mailto:tech@origin-trail.com>`__\ .

Nodes can be installed in two ways:

-  via docker, which is recommended way, also explained on our website

-  manually

NOTE: For best performance on running a node we recommend usage of services like Digital Ocean.

Prerequisites
~~~~~~~~~~~~~

System requirements

-  minimum of 2Gb of RAM memory

-  at least 1 CPU

-  at least 10GB storage space

-  Ethereum wallet (You can see wallet setup instructions here Identity Configuration)

-  for testnet node: at least 1000 test TRAC tokens and at least 0.05 test Ethers

-  for mainnet node: at least 1000 TRAC tokens and at least 0.05 Ethers

Installation via Docker
~~~~~~~~~~~~~~~~~~~~~~~~

Prerequisites
+++++++++++++++
Public IP or open communication

A public IP address, domain name, or open network communication with the Internet is required. If behind NAT, please manually setup port forwarding to all the node’s ports.

Docker installed
++++++++++++++++

The host machine needs to have Docker installed to be able to run the Docker commands specified below. You can find instructions on how to install Docker here:

For Mac https://docs.docker.com/docker-for-mac/install/

For Windows https://docs.docker.com/docker-for-windows/install/

For Ubuntu https://docs.docker.com/install/linux/docker-ce/ubuntu/

It is strongly suggested to use the latest official version.

Open Ports
++++++++++

By default Docker container will use 8900, 5278 and 3000 ports. These can be mapped differently in Docker container initialization command. Make sure they’re not blocked by your firewall and that they are open to the public.

Please note: port 8900 is used for REST API access which is not available until OT node is fully started. This can be concluded after the following log message is displayed in the running node.

    ``info - OT Node started``

Installation
++++++++++++

Before running a node make sure you configure it properly first. You can proceed to the Node Configuration page.

Run a node on the MAINNET
+++++++++++++++++++++++++


Let’s just point Docker to the right image and configuration file with the following command:

.. code:: bash

    sudo docker run -i --log-driver json-file --log-opt max-size=1g --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc quay.io/origintrail/otnode:release_mainnet

NOTE: Please make sure that your ``.origintrail_noderc`` file is ready before running the following commands. In this example, the configuration file ``.origintrail_noderc`` is placed into the home folder of the current user (ie. /home/ubuntu). You should point to the path where you created ``.origintrail_noderc`` on your file system.

Run a node on the TESTNET
+++++++++++++++++++++++++

Let’s just point Docker to the right image and configuration file with the following command:

.. code:: bash

    sudo docker run -i --log-driver json-file --log-opt max-size=1g --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc quay.io/origintrail/otnode:release_testnet

NOTE: Please make sure that your ``.origintrail_noderc`` file is ready before running the following commands. In this example, the configuration file ``.origintrail_noderc`` is placed into the home folder of the current user (ie. /home/ubuntu). You should point to the path where you created ``.origintrail_noderc`` on your file system.

Manual installation
~~~~~~~~~~~~~~~~~~~

Prerequisites
++++++++++++++

NodeJS
++++++

If you don’t have Node.js installed head to https://nodejs.org/en/ and install version 9.x.x.

Note: Make sure you have the precisely above specified version of Node.js installed. Some features will not work well on versions less or greater then 9.x.x.

Before starting, make sure your server is up-to-date. You can do this with the following commands:

.. code:: bash

    curl -sL https://deb.nodesource.com/setup\_9.x | sudo -E bash
    sudo apt-get install -y nodejs

Database - ArangoDB
+++++++++++++++++++

ArangoDB is a native multi-model, open-source database with flexible data models for documents, graphs, and key-values. We are using ArangoDB to store data. In order to run OT node with ArangoDB you need to have a local ArangoDB server installed and running.

Head to arangodb.com/download, select your operating system and download ArangoDB. You may also follow the instructions on how to install with a package manager, if available. Remember credentials (username and password) used to log in to Arango server, since later on you will need to set them in ``.origintrail_noderc`` \ .

Installation
++++++++++++

Clone the repository

.. code:: bash

    git clone -b release/mainnet https://github.com/OriginTrail/ot-node.git

in the root folder of a project (ot-node), create ``.env`` file. For manually running a mainnet node, add following variable in .env file:

    ``NODE_ENV=mainnet``

or for manually running a testnet node,

    ``NODE_ENV=testnet``

Before running a node make sure you configure it properly first. You can proceed to node Node Configuration page.

and then run npm from root project folder

.. code:: bash

    cd ot-node
    npm install
    npm run setup

Starting The Node
++++++++++++++++++

OT node consists of two servers RPC and Kademlia node. Run both servers in a single command.

    ``npm start``

You can see instructions regarding the data import on the following Import data

Important Notes
~~~~~~~~~~~~~~~

Before running your node for the first time you need to execute npm run setup to apply the  initial configuration.

If you want to reset all settings you can use npm run setup:hard. If you want to clear all the cache and recreate the database and not delete your identity just run npm run setup.

In order to make the initial import, your node must whitelist the IP or host of the machine that is requesting the import in configuration i.e

.. code:: json

    {
        "network": {
            "remoteWhitelist": [ "host.domain.com", "127.0.0.1"]
        }
    }

By default only localhost is whitelisted.

For more information see Node Configuration.

Useful commands
~~~~~~~~~~~~~~~

Check node status
++++++++++++++++

To check if your node is running in Terminal, run the following command:

    ``docker ps -a``

This command will indicate if your node is running.

Starting OT Node
++++++++++++++++

This command will start your node as a background process.

    ``docker start otnode``

This command will start your node in interactive mode and you will see the node’s process written in the terminal, but this command will not run your node as a background process, which means your node will stop if you close your Terminal/Console.

    ``docker start -i otnode``

Stopping OT Node
++++++++++++++++

You can stop your node in the following two ways:

If you started your node with the docker start otnode command and you wish to stop it from running, use the following command in your terminal:

    ``docker stop otnode``

If you started your node by using the docker start -i otnode command, you can stop it either by closing the Terminal or simply by pressing the ctrl + c.

Configuration
-------------

Prerequisites
~~~~~~~~~~~~~~

There’s a minimum set of config parameters that need to be provided in order to run a node, without which the node will refuse to start.

Basic configuration
~~~~~~~~~~~~~~~~~~~~

To properly configure the node you will need to create a config file in JSON format and provide some basic parameters for node operation. This file will be consumed by node upon start. Let’s create the file .origintrail\_noderc in OT node root dir and store all the information about what kind of configuration we want to set up. The bare minimum of settings that needs to be provided is two valid Ethereum wallet addresses: - for the operational wallet (OW), which maps to node\_wallet (OW public address) and node\_private\_key (OW private key) - for the management wallet provide a public Ethereum address of your management wallet in the “management\_wallet” parameter

You also need to provide a public address or domain name.

We create the .origintrail\_noderc file with following content:

.. code:: json

    {
        "node_wallet": "your wallet address here",
        "node_private_key": "your wallet's private key here",
        "management_wallet": "your management wallet public key here",
        "network": {
            "hostname": "your external IP or domain name here",
            "remoteWhitelist": [ "IP or host of the machine that is requesting the import", "127.0.0.1"]
        },
        "blockchain": {
        "rpc_server_url": "url to your RPC server i.e. Infura or own Geth"
        }
    }

``node_wallet`` and ``node_private_key`` - operational wallet Ethereum wallet address and its private key.

``management_wallet`` - the management wallet for your node (note: the Management wallet private key is NOT stored on the node)

``hostname`` - the public network address or hostname that will be used in P2P communication with other nodes for node’s self identification.

``remoteWhitelist`` - list of IPs or hosts of the machines (“host.domain.com”) that are allowed to communicate with REST API.

``rpc_server_url`` - an URL to RPC host server, usually Infura or own hosted Geth server. For more see RPC server host

Configuration file
~~~~~~~~~~~~~~~~~~

In general OT node uses [RC](https://www.npmjs.com/package/rc) nodejs package to load configuration and everything mentioned there applies to the OT node.

Application name that will be used in detecting the config files is origintrail\_node. Translated from RC package page a configuration file lookup will be like this (from bottom towards top):

command line arguments, parsed by minimist (e.g. –foo baz, also nested: –foo.bar=baz)

environment variables prefixed with origintrail\_node\_

or use “\_\_” to indicate nested properties (e.g. origintrail\_node\_foo\_\_bar\_\_baz => foo.bar.baz)

if you passed an option –config file then from that file

a local .origintrail\_noderc or the first found looking in ./ ../ ../../ ../../../ etc.

 - $HOME/.origintrail\_noderc

 - $HOME/.origintrail\_node/config

 - $HOME/.config/origintrail\_node

 - $HOME/.config/origintrail\_node/config

 - /etc/origintrail\_noderc

 - /etc/origintrail\_node/config

the defaults object you passed in.

All configuration sources that were found will be flattened into one object, so that sources earlier in this list override later ones.

NOTE: To see all configuration parameters and their default values you can check this link:

`https://github.com/OriginTrail/ot-node/blob/develop/config/config.json <https://github.com/OriginTrail/ot-node/blob/develop/config/config.json>`__

Setting up Ethereum RPC
-----------------------

For an OT node to run it must communicate with the Ethereum blockchain. Such communication is achieved using the Ethereum JSON RPC protocol and a RPC compatible server.

RPC server configuration
~~~~~~~~~~~~~~~~~~~~~~~~

The RPC server URL must be provided in the OT node’s configuration file and it should be placed in the blockchain section as rpc\_server\_url. For example:

.. code:: json

    "blockchain": {
        "rpc_server_url": "https://my.rpc.server.url:9000/"
    }

For more on how to set configuration file go to Node Configuration

Using Infura as RPC host

Using Infura gives a lot of advantages such as not needing to host your own server or configuring the Ethereum node client or even not scaling the whole infrastructure.

In order to use it create an account at https://infura.io . Once logged-in you can create a project for which you’ll have project ID, project secret and the endpoint. That endpoint is the RPC server URL needed for the node to run. Make sure you pick the right one for the target network. Select RINKEBY to get the URL that will be used in the Testnet or MAINNET for the OriginTrail’s mainnet.

Using own Ethereum node as RPC host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To use the Ethereum node as an RPC server make sure it is properly configured and RPC feature is enabled (–rpc parameter). For more details on how to install and configure Ethereum node see: https://github.com/ethereum/go-ethereum/wiki/Installing-Geth .

Once the Ethereum node is up and running use its URL to point to the OT node to use it.

Setting up SSL on a node
------------------------

Before you begin setting up an SSL connection for a node’s remote API, make sure you have prepared certificates and registered a domain. Once you have enabled a secure connection, it will be used for both API (default port 8900) and remote control (default port 3000). If you are using different ports than the defaults, make sure you map them correctly during container initialization.

Prerequisites
~~~~~~~~~~~~~

Make sure your certificates are in PEM format and stored locally, as you will need to provide them to the node or Docker container running the node.

Configuration
~~~~~~~~~~~~~

Let’s assume that your domain certificates (for example: my.domain.com) are stored in /home/user/certs. The fullchain.pem and privkey.pem files should be in that dir.

Edit the node’s configuration file and make sure it has the following items in the JSON root:

.. code:: json

    "node_rpc_use_ssl": true,
    "node_rpc_ssl_cert_path": "/ot-node/certs/fullchain.pem",
    "node_rpc_ssl_key_path": "/ot-node/certs/privkey.pem",

With the above, we are telling the node to find a certificate at the following path: /ot-node/certs/. That is where we are going to leave them in the container.

Now, create the docker container and mount cert dir into the container. We can achieve this by adding additional parameters ‘-v /home/user/certs:/ot-node/certs/’ to the container creation command. For example, the initialization of the Docker container for the OT node for the mainnet could look like this:

.. code:: bash

    sudo docker run -i --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v /home/user/certs:/ot-node/certs/ -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc quay.io/origintrail/otnode:release_mainnet

After this, the running container will be able to find certificate files at the ‘/ot-node/certs/’ location.

How to update
-------------

OT Node has a built-in update functionality which will be triggered upon OT Node start.

Docker
~~~~~~

In order to trigger the update, you must restart the OT Node by using the following command:

.. code:: bash

    docker restart otnode

After a successful update OT Node will be rebooted automatically.

NOTE: By default node comes with the  auto update feature turned on (it can be turned off using configuration). If auto update is on, Node checks for the update every 6 hours and it will automatically download and install the newest version when it’s available. Without need for manual restart.

Manual installation
~~~~~~~~~~~~~~~~~~~~

Make sure that you are in the root directory of OT Node. The following commands will update the OT Node.

.. code:: bash

    git pull
    docker stop otnode

Database migrations need to be triggered manually.

.. code:: bash

    node_modules/.bin/sequelize --config=./config/sequelizeConfig.js db:migrate

Database seed needs to be triggered manually as well.

.. code:: bash

    node_modules/.bin/sequelize --config=./config/sequelizeConfig.js db:seed

In order to apply the update, you must restart the OT Node by using the following command:

.. code:: bash

    docker start otnode