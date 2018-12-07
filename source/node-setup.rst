..  _node-setup:

Installation
============

Read Me First
-------------

Please keep in mind that we will give our best to support you while setting up and
testing the nodes. Some features are subject to change and we are aware that some defects may show up on
different installations and usage scenarios. 

If you need help installing OT Node or troubleshooting your
installation, you can either:

- engage in our Discord community and post your question, 
- contact us directly via email at support@origin-trail.com.

Nodes can be installed in two ways:

- via docker, which is reccommended way, also explained on our `website`_ 
- manually

**NOTE**: For best performance on running node we recommend usage of services like Digital Ocean.

In order to manually install and run OT node, you should do following steps:

1. **Step 1** - Install all prerequisites.
   
2. **Step 2** - :ref:`configuration-setup`

3. **Step 3** - :ref:`import-data`

Prerequisites
-------------

System requirements
~~~~~~~~~~~~~~~~~~~
-  minimum of 2Gb of RAM memory
-  at least 1 CPU
-  at least 10GB storage space 
-  Ethereum wallet (You can see wallet setup instructions here :ref:`wallet-setup`)
-  for testnet node: at least 1000 test TRAC tokens and at least 0.01 test Ethers 
-  for mainnet node: at least 1000 TRAC tokens and at least 0.01 Ethers


Installation via Docker Prerequisites
---------------------------------------

Public IP or open communication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A public IP address, domain name, or open network communication with the Internet is required. If behind NAT, please manually setup port forwarding to all the node’s ports.

Docker installed
~~~~~~~~~~~~~~~~~~~
The host machine needs to have Docker installed to be able to run the Docker commands specified below. You can find instructions on how to install Docker here:

For Mac https://docs.docker.com/docker-for-mac/install/ 

For Windows https://docs.docker.com/docker-for-windows/install/ 

For Ubuntu https://docs.docker.com/install/linux/docker-ce/ubuntu/ 

It is strongly suggested to use the latest official version.

Open Ports
~~~~~~~~~~~

By default Docker container will use 8900, 5278 and 3000 ports.
These can be mapped differently in Docker container initialization command.
Make sure they’re not blocked by firewall and open to the public.

Please note: port 8900 is used for REST API access which is not available until OT node is fully started. 
This can be concluded after following log message is displayed in running node.

info - OT Node started


Installation via Docker
------------------------

Before running a node make sure you configure it properly first. You can proceed to node :ref:`Configuration-setup` page.

Run a node on the TESTNET
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Let’s just point Docker to the right image and configuration file with the following command:

.. code:: bash

    sudo docker run -i --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc origintrail/ot-node

NOTE: In this example, our configuration file .origintrail_noderc is placed into the home folder of the current user (ie. /home/ubuntu).
You should point to the path where you created .origintrail_noderc on your file system.


Run a node on the MAINNET
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Let’s just point Docker to the right image and configuration file with the following command:

.. code:: bash

    sudo docker run -i --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc quay.io/origintrail/otnode-mariner:release_mariner

NOTE: In this example, our configuration file .origintrail_noderc is placed into the home folder of the current user (ie. /home/ubuntu).
You should point to the path where you created .origintrail_noderc on your file system.


Check node status
~~~~~~~~~~~~~~~~~~~~~
To check if your node is running in Terminal, run the following command: 

.. code:: bash
    
    docker ps -a

This command will indicate if your node is running.

Starting OT Node
~~~~~~~~~~~~~~~~~~~~~

This command will start your node as a background process.

.. code:: bash

    docker start otnode

This command will start your node in interactive mode and you will see the node’s process written in the terminal,
but this command will not run your node as a background process, which means your node will stop if you close your Terminal/Console.

.. code:: bash
    
    docker start -i otnode

Stopping OT Node
~~~~~~~~~~~~~~~~~~~~~
You can stop your node in the following two ways:

If you started your node with the docker start otnode command and 
you wish to stop it from running, use the following command in your terminal: 

.. code:: bash

    docker stop otnode

If you started your node by using the docker start -i otnode command, 
you can stop it either by closing the Terminal or simply by pressing the ctrl + c.


.. _-manual-prerequisites-installation:

Manual Installation Prerequisites 
---------------------------------------

NodeJS
~~~~~~~

If you don't have Node.js installed head to https://nodejs.org/en/ and
install version 9.x.x.

**Note:** Make sure you have the preciselly above specified version of
Node.js installed. Some features will not work well on versions less or
greater then 9.x.x.

Before starting, make sure your server is up-to-date. You can do this
with the following commands:

.. code:: bash

   curl -sL https://deb.nodesource.com/setup_9.x | sudo -E bash -
   sudo apt-get install -y nodejs

Database - ArangoDB
~~~~~~~~~~~~~~~~~~~~~~

**ArangoDB** is a native multi-model, open-source database with flexible
data models for documents, graphs, and key-values. We are using ArangoDB
to store data. In order to run OT node with ArangoDB you need to have a
local ArangoDB server installed and running.

Head to `arangodb.com/download`_, select your operating system and
download ArangoDB. You may also follow the instructions on how to
install with a package manager, if available. Remember credentials
(username and password) used to log in to Arango server, since later on
you will need to set them in .origintrail_noderc.

.. _ubuntu-1604:

Ubuntu 16.04
~~~~~~~~~~~~~~~~~~~

.. code:: bash

    curl -OL https://download.arangodb.com/arangodb33/xUbuntu_16.04/Release.key
    apt-key add - < Release.key
    echo 'deb https://download.arangodb.com/arangodb33/xUbuntu_16.04/ /' | tee /etc/apt/sources.list.d/arangodb.list
    apt-get install apt-transport-https -y
    apt-get update -y
    echo arangodb3 arangodb3/backup boolean false | debconf-set-selections
    echo arangodb3 arangodb3/upgrade boolean true | debconf-set-selections
    echo arangodb3 arangodb3/storage_engine select mmfiles | debconf-set-selections
    echo arangodb3 arangodb3/password password root | debconf-set-selections
    echo arangodb3 arangodb3/password_again password root | debconf-set-selections
    apt-get install arangodb3=3.3.12 -y

When asked, enter the password for root user.

Mac OS X
~~~~~~~~~~~~~~~~~~~

For Mac OS X, you can use **homebrew** to install ArangoDB. Run the
following:

.. code:: bash

   brew install arangodb

.. _ubuntu-1604-1:


Manual Node Installation
-------------------------

Clone the repository

.. code:: bash

   git clone -b master https://github.com/OriginTrail/ot-node.git

in the root folder of a project (ot-node), create .env file.
For manually running a testnet node, add follwoing variable in .env file:

NODE_ENV=production

or for manually running a mainnet node, 

NODE_ENV=mariner

Before running a node make sure you configure it properly first. You can proceed to node :ref:`Configuration-setup` page.

and then run npm from root project folder

.. code:: bash

   cd ot-node
   npm install
   npm run setup
   

Starting The Node
~~~~~~~~~~~~~~~~~~~

OT node consists of two servers **RPC** and **Kademlia node**. Run both
servers in a single command.

.. code:: bash

   npm start

You can see instructions regarding the data import on the following :ref:`import-data`

Important Notes
~~~~~~~~~~~~~~~~~~~

First time you run your node run ``npm run setup`` to apply initial configuration.

If you want to reset all settings you can use ``npm run setup:hard``. If you want to
clear all the cache and recreate database and not delete identity just run ``npm run setup``.

In order to make the initial import, your node must **whitelist** the
IP or host of the machine that is requesting the import in configuration i.e

.. code:: json

    {
        "network": {
            "remoteWhitelist": [ "host.domain.com", "127.0.0.1"]
        }
    }

By default only localhost is whitelisted.

For more information see :ref:`Configuration-setup`.


.. _Issues: https://github.com/OriginTrail/ot-node/issues
.. _manually: #manual
.. _website: https://www.origintrail.io/node-setup
.. _arangodb.com/download: https://www.arangodb.com/download-major/
.. _link: https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04
