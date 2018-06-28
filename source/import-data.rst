..  _import-data:

Import data
======================================

In order to import data it has to be properly formatted. Further
description of XML schema and sample files can be found in :ref:`data-structure-guidelines`. 

We included `example`_ files in the project for your reference and
testing. Please take a look `here`_ for GS1 EPCIS XML schema.

To import data from the XML file into OriginTrail, send a HTTPS POST
request containing the XML file to the following endpoint:

::

   https://YOUR_RPC_NODE_URL:YOUR_RPC_NODE_PORT/import_gs1

The example cURL request is:

.. code:: bash

   curl -v  -F importfile=@importers/xml_examples/Basic/01_Green_to_pink_shipment.xml        http://YOUR_RPC_NODE_URL:YOUR_RPC_NODE_PORT/import_gs1

Depending on the use case, it might make sense to set up a periodic
import of the data into OriginTrail (i.e. a daily cronjob exporting the
data from your ERP in the XML format requested, with a POST to your OT
node).

What happens after the successful import of the data?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can perform import on any of the two nodes. Node that you import to
will act as DC (Data Creator) node, as we refer to it in our
documentation, while other one will act as DH (Data Holder).

After you make the import you will see the full process of two nodes
doing their routine. Simplified, this is what will happen:

1. First the DC node will fingerprint the data import on Blockchain and
   then create the offer and broadcast it to other nodes on network.
2. Other nodes (in our case DH) will answer and send their own encrypted
   bids.
3. Once the bidding process is over, nodes will reveal their bids and
   Smart Contract will choose the nodes that are selected to do the
   replication.
4. Once the DH is chosen, it will receive the replication request
   containing the data of the import.
5. DH will store the data while DC will generate and start sending
   random challenges and DH will answer them if it still has the data.
6. Once the period of job is finished (or before for the job already
   done) DH may choose to claim it's fee in Alpha TRAC.

This is quite simplified flow, but you will be able to follow it in
terminal as it happens. Also, you may directly access the graph database
and check if both nodes have the data imported.

If the data is in the database and you have spent some ATRAC for DH
services, you may consider your installation successful.

.. _example: https://github.com/OriginTrail/ot-node/tree/develop/importers/xml_examples
.. _here: https://github.com/OriginTrail/ot-node/tree/develop/importers/xsd_schemas
