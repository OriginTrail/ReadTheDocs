Dataset operations
======================

Importing datasets to the local node knowledge graph
----------------------------------------------------

The ODN supports a multitude of data models (as listed below), with extensible support due to the flexible native
OT-JSON data structure supported by the protocol, based on JSON-LD. 

To introduce a structured dataset to the network, we first need to import it to the node's local knowledge graph using
the import command. This command initiates the import process and returns an internally-generated handler ID, which is
a UUID. Using this handler ID, you can check the status of the import process, which can take a certain amount of time,
depending on the input dataset size. If an import fails for any reason, the data field of the import result response
will contain an error message and the status field will have the value “FAILED”.

The import API details are explained on the following link

`https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/import <https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/import>`__

Supported standards
^^^^^^^^^^^^^^^^^^^

The OriginTrail node supports the following standard data models for importing and connecting data in its
Decentralized Knowledge Graph. This is by no means a definite list, as the development community and core development
team are extending the support for multiple standards on an ongoing basis.


+----------------+----------------+----------------+----------------+----------------+
| Standard /     | Standardization| Import         | Description    | Official       |
| Data model     | body           | \ **Standard   |                | documentation  |
| name           |                | ID** to be     |                |                |
|                |                | used           |                |                |
+----------------+----------------+----------------+----------------+----------------+
| CBV            | GS1            | GS1-EPCIS      | Supported      | `Core Business |
|                |                |                | within         | Vocabulary <ht |
|                |                |                | standard EPCIS | tps://www.gs1. |
|                |                |                | XML structure  | org/sites/defa |
|                |                |                |                | ult/files/docs |
|                |                |                |                | /epc/CBV-Stand |
|                |                |                |                | ard-1-2-r-2016 |
|                |                |                |                | -09-29.pdf>`__ |
+----------------+----------------+----------------+----------------+----------------+
| EPCIS 1.2      | GS1            | GS1-EPCIS      | Supported      | `EPCIS         |
|                |                |                | within         | standard       |
|                |                |                | standard EPCIS | documentation  |
|                |                |                | XML structure  | <https://www.g |
|                |                |                |                | s1.org/sites/d |
|                |                |                |                | efault/files/d |
|                |                |                |                | ocs/epc/EPCIS- |
|                |                |                |                | Standard-1.2-r |
|                |                |                |                | -2016-09-29.pd |
|                |                |                |                | f>`__          |
+----------------+----------------+----------------+----------------+----------------+
| ID keys        | GS1            | GS1-EPCIS      | Supported      | `GS1           |
| (GIAI,GRAI,    |                |                | within         | Keys <https:// |
| GTIN etc)      |                |                | standard EPCIS | www.gs1.org/st |
|                |                |                | XML structure  | andards/id-key |
|                |                |                |                | s>`__          |
+----------------+----------------+----------------+----------------+----------------+
| Web of Things  | W3C            | WOT            | Supporting     | `Web of Things |
|                |                |                | native Web of  | documentation  |
|                |                |                | Things JSON    | <https://www.w |
|                |                |                | data model     | 3.org/Submissi |
|                |                |                |                | on/wot-model>` |
|                |                |                |                | __             |
+----------------+----------------+----------------+----------------+----------------+
| Verifiable     | W3C            | OT-JSON        | Supported      | `Verifiable    |
| Credentials    |                |                | through        | Credentials    |
|                |                |                | protocol       | Data           |
|                |                |                | native OT-JSON | model <https:/ |
|                |                |                | file structure | /www.w3.org/TR |
|                |                |                |                | /vc-data-model |
|                |                |                |                | />`__          |
+----------------+----------------+----------------+----------------+----------------+
| PROV           | W3C            | OT-JSON        | Supported      | `W3C           |
|                |                |                | through        | Provenance     |
|                |                |                | protocol       | data           |
|                |                |                | native OT-JSON | model <https:/ |
|                |                |                | file structure | /www.w3.org/TR |
|                |                |                |                | /prov-dm/>`__  |
+----------------+----------------+----------------+----------------+----------------+

 

The following API route returns a list of supported **standard ID**\ s:

`https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/info/get\_standards <https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/info/get_standards>`__


Replicating datasets across the ODN
-----------------------------------

In order to replicate a dataset to the network, it first has to be imported to the local knowledge graph on your node,
in order for it to be properly structured and prepared for the network replication process. To initiate the replication
process,  utilize the \ *replicate*\  API call. This command will create a network data holding offer on the ODN and the
node will start a negotiation process with other nodes using the protocol’s incentivized replication procedure. If the
replication fails to be completed for any reason, the data field in the replication result call will contain an
appropriate error message, and the status field will have the value “FAILED”. Once when replication process is complete
the ODN offer details are written on the blockchain.

The replication API details are explained on the following link

`https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/replicate <https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/replicate>`__

Verification and signature checks
---------------------------------

When importing a dataset, the graph representation of the dataset is hashed to generate a unique identifier for that
dataset, called the ``dataset_id``\ .

The entire dataset (graph data together with dataset metadata) is used to generate a
`Merkle tree <https://en.wikipedia.org/wiki/Merkle_tree>`__ of the dataset, and the root hash of the Merkle tree is
considered to be the dataset **root hash**\ . This process ensures data integrity because changing any part of the
dataset would cause the dataset **root hash** to change.

When a dataset is replicated on the network, the integrity of the dataset can be verified by fetching the Merkle tree
root hash from the blockchain (published during the replication procedure). 

The fingerprint API route returns the Merkle tree root hash from the blockchain of the dataset with the given
``dataset_id``\ . The fingerprint API details are explained on the following link

`https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/info/get\_fingerprint\_\_id <https://app.swaggerhub.com/apis-docs/otteam/ot-node-api/v2.0#/info/get_fingerprint__id_>`__