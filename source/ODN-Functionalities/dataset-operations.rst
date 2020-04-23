Dataset operations
======================

Importing datasets to the local node knowledge graph
------

ODN supports a multitude of data models (i.e. supply chain standards such as G1 EPCIS and W3C Web of Things),
with extensible support due to the flexible native OT-JSON data model supported by the protocol, based on JSON-LD.
To introduce this dataset to the network, we first need to import it to the node using the import command.
This command initiates the import process and returns an internally-generated handler ID, which is a UUID.
Using this handler ID, you can check the status of the import process, which can take a certain amount of time,
depending on the file size. If an import fails to complete for any reason, the data field will contain the error
message and the status field will be marked as FAILED.

------------

Swagger API import details are available on the following link:

`https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/import <https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/import>`__

Replicating datasets across the ODN
-----------

In order to replicate the imported dataset on the network, trigger the replication process by calling the replicate API
call. With this command, you will initiate an offer on the ODN and your node will start negotiating with other nodes on
the replication procedure.If the replication fails to be completed for any reason, the data field will contain the
error message, and the status field will be FAILED. Once when replication is completed,
offer details are written on the blockchain.

------------

Swagger API import details are available on the following link:

`https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/replicate <https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/replicate>`__

Verification and signature checks
---------------------------------

When a dataset is replicated on the network, you can check dataset integrity by fetching root hash from the blockchain.
Fingerprint API route returns root hash from the blockchain of the dataset with specific identifier.

------------
 

Swagger API import details are available on the following link:

`https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/info/get\_fingerprint\_\_id <https://app.swaggerhub.com/apis/otteam/ot-node-api/v2.0#/info/get_fingerprint__id_>`__


