..  _configuration-setup:

Node Configuration
======================================

.env File Configuration
---------------------------

Go into ot-node folder (if you are not already there) with
``cd ot-node``

Open ``.env`` in your favorite editor and set the configuration
parameters or type this in console: ``nano .env``

Make sure that username and password you set when installing your
preferred database are set as values of respective database USERNAME and
PASSWORD.

You can view .env Example `here`_

Once you set your .env file, you have to run from ot-node folder decide on one of following options:

- if this is your initial configuration: 

::

   npm run bootstrap

- if you are updating node setup (node was working already):

::

   npm run config

Every time you change your configuration in .env don't forget to run
this command to apply that configuration.

.. _here: https://github.com/OriginTrail/ot-node/blob/develop/.env.example
