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

- via docker, which is reccommended way, explained on our `website`_ 
- manually ( to be explained below )

**NOTE**: For best performance on running node we recommend usage of services like Digital Ocean.

In order to manually install and run OT node, you should do following steps:

1. **Step 1** - Install all prerequisites.
   
2. **Step 2** - :ref:`configuration-setup`

3. **Step 3** - :ref:`import-data`

Prerequisites
-------------

System requirements
~~~~~~~~~~~~~~~~~~~
-  Minimum of 2Gb of RAM memory
-  Minimum of 5Gb of storage memory 
-  Ethereum wallet and some Ether on Rinkeby Test Network (You can see wallet setup instructions here :ref:`wallet-setup`)



.. _-manual-prerequisites-installation:

Manual Prerequisites Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NodeJS
^^^^^^

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
^^^^^^^^^^^^^^^^^^^

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
************

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
********

For Mac OS X, you can use **homebrew** to install ArangoDB. Run the
following:

.. code:: bash

   brew install arangodb

.. _ubuntu-1604-1:


Manual Node Installation
------------------------

Clone the repository

.. code:: bash

   git clone -b master https://github.com/OriginTrail/ot-node.git

and run npm

.. code:: bash

   cd ot-node
   npm install
   npm run setup
   
Before running a node make sure you configure it properly first. You can proceed to node :ref:`Configuration-setup` page.

Starting The Node
-----------------

OT node consists of two servers **RPC** and **Kademlia node**. Run both
servers in a single command.

.. code:: bash

   npm start

You can see instructions regarding the data import on the following :ref:`import-data`

Important Notes
---------------

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
