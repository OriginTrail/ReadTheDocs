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

Once you set your .env file, you have to run from ot-node folder:

::

   npm run config


.. _here: https://github.com/OriginTrail/ot-node/blob/develop/.env.example
