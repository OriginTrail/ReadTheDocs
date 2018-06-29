..  _import-data:

Import data
======================================

In order to make the initial import there are prerequisits:

-  your node must whitelist the IP of the machine that is requesting the import in .env i.e IMPORT_WHITELIST=127.0.0.1 if you are importing from localhost.
-  data has to be properly formatted. Further description of XML schema and sample files can be found in :ref:`data-structure-guidelines`. 
- We included `example`_ files in the project for your reference and testing. 

To import data from the XML file into OriginTrail, send a HTTPS POST
request containing the XML file to the following endpoint:

::

   https://YOUR_RPC_NODE_URL:YOUR_RPC_NODE_PORT/import_gs1

The example cURL request is:

.. code:: bash

   curl -v  -F importfile=@importers/xml_examples/Basic/01_Green_to_pink_shipment.xml http://YOUR_RPC_NODE_URL:YOUR_RPC_NODE_PORT/import_gs1

Depending on the use case, it might make sense to set up a periodic
import of the data into OriginTrail (i.e. a daily cronjob exporting the
data from your ERP in the XML format requested, with a POST to your OT
node).

.. _example: https://github.com/OriginTrail/ot-node/tree/develop/importers/xml_examples
