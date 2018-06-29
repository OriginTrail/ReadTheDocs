..  _node-setup:

Node Setup instructions
========================

Read Me First
-------------

Please bear in mind that we are only able to give you limited support
on testing the nodes, some features will probably change and we are aware of some bugs that may show up on
different installation and usage scenarios. If you need help installing OT Node or troubleshooting your
installation, you can contact us directly via email at support@origin-trail.com.


Nodes can be installed on several ways:

- via docker https://www.origintrail.io/node-setup
- automatic installation on Ubuntu (explained below)
- manual installation (explained below)

**NOTE**: For best performance testing we recommend usage of services like Digital Ocean.

In order to install OT node, you should do following steps:

1. **Step 1** - Install all prerequisites. There is an automatic
   installation script or you can do installation manually (explained below).
   
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

   sudo apt-get update -y
   sudo apt-get upgrade -y

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
you will need to set them in .env.

.. _ubuntu-1604:

Ubuntu 16.04
************

.. code:: bash

   wget https://www.arangodb.com/repositories/arangodb3/xUbuntu_16.04/Release.key
   sudo apt-key add Release.key
   sudo apt-add-repository 'deb https://www.arangodb.com/repositories/arangodb3/xUbuntu_16.04/ /'
   sudo apt-get update -y
   sudo apt-get install arangodb3

When asked, enter the password for root user.

Mac Os X
********

For Mac OS X, you can use **homebrew** to install ArangoDB. Run the
following:

.. code:: bash

   brew install arangodb

Database Setup
^^^^^^^^^^^^^^

Once you installed ArangoDB you should create a database. Enter ArangoDB
shell script

.. code:: bash

   arangosh

and create database

.. code:: javascript

   db._createDatabase("origintrail", "", [{ username: "otuser", passwd: "otpass", active: true}])

Database - Neo4j
^^^^^^^^^^^^^^^^

**Neo4j** is a graph database management system with native graph
storage and processing. Its architecture is designed for optimizing fast
management, storage, and the traversal of nodes and relationships. In
order to run OT node with Neo4j make sure to have it installed and
running.

Head to `neo4j.com/download`_, select your operating system and download
Neo4j. You may also follow the instructions on how to install with a
package manager, if available.

.. _ubuntu-1604-1:

Ubuntu 16.04
************

First you have to install Java 8 and set it as the default.

.. code:: bash

   sudo add-apt-repository ppa:webupd8team/java
   sudo apt-get update
   sudo apt-get install oracle-java8-installer
   sudo apt-get install -y oracle-java8-set-default

Run the following:

::

   wget -O - https://debian.neo4j.org/neotechnology.gpg.key | sudo apt-key add -
   echo 'deb https://debian.neo4j.org/repo stable/' | sudo tee /etc/apt/sources.list.d/neo4j.list
   sudo apt-get update
   sudo apt-g

Automatic installation
----------------------

This will install all prerequisites in a single step.

.. code:: bash

   wget https://raw.githubusercontent.com/OriginTrail/ot-node/master/install.sh
   sh install.sh --db=arangodb

If you prefer neo4j as database then use

::
   +

**Note:** There are some ongoing issues with Neo4j. We currently advise use of ArangoDB.

If errors occurred during installation process, ot-node probably won't
work properly. Errors during installation process happen due to various
factors like lack of RAM or previous installations. We strongly
recommend installation on clean system and at least 2GB of RAM (it may work with 512MB and swap file).
You can check this `link`_ and do the automatic installation and setup again. 

If you used this automatic installation script, you may proceed to :ref:`configuration-setup`. Then you can start the node.

Manual Node Installation
------------------------

Clone the repository

.. code:: bash

   git clone -b master https://github.com/OriginTrail/ot-node.git

and run npm

.. code:: bash

   cd ot-node && npm install
   cp .env.example .env

Starting The Node
--------------------

OT node consists of two servers **RPC** and **Kademlia node**. Run both
servers in a single command.

.. code:: bash

   npm start

You can see instructions regarding the data import on the following :ref:`import-data`

Important Notes
-----------------

Every time you change your configuration in .env don't forget to run
``npm run config`` to apply that configuration.

In order to make the initial import, your node must **whitelist** the
IP of the machine that is requesting the import in ``.env`` i.e
IMPORT_WHITELIST=127.0.0.1 if you are importing from localhost.


.. _Issues: https://github.com/OriginTrail/ot-node/issues
.. _manually: #manual
.. _neo4j.com/download: https://neo4j.com/download/
.. _arangodb.com/download: https://www.arangodb.com/download-major/
.. _link: https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04
