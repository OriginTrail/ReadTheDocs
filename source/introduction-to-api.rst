..  _introduction-to-api:


Introduction to API
========================

The purpose of this API is to allow data operations on a single node you trust, in order to control the data flow via API routes. For example, importing a single data file into a node's database, replicating the data on the network or reading it from the node.


Prerequisites
========================

Your node must be running and you must have a properly set up a listening address and import address whitelist.
Your listening address, as well as the import whitelist can be set in the .env file before running the node for the first time.
For instructions on how to configure the node, check :ref:`configuration-setup` page.

Import
============

Importing the data to ODN is executed in several steps:

1. **/api/import** with data as parameter is importing the data from data source to local graph database on DC node. Data import_id is returned to data creator as a parameter in response.
2. **/api/replication/** with import_id as input parameter is replicating the data from local graph database on DC node to other DH nodes via bidding mechanism.

After all steps are completed, imported data can be read from ODN via `read`_ procedure.

/api/import ``POST``
-------------------------

Import new GS1 or WOT structured data into a node's database. Find out more about data format here :ref:`data-structure-guidelines`
 
 
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``POST`` http://NODE_IP:PORT/api/import
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "importfile", "true", "file or text", "data that you want to import (ex. GS1 XML file)"
   "importtype", "true", "predetermined text", "GS1 or WOT. This describes data standard."
  
If successful returns import_id for this data import.

*Example:*

::

    {
        "importfile": "importfile=@/ot-node/test/modules/test_xml/GraphExample_1.xml"
        "importtype": "GS1"
    }

Responses
~~~~~~~~~~~~~~

``201`` File was successfully imported.


*Example:*

::

    {
        "import_id": "0x477eae0227cce0ffaadc235c7946b97cbe2a948fe7782796b53a0c5a6ca6595f"
    }

``400`` Invalid import parameters (importfile/importtype)

*Example:*

::

    {
         "message": "Invalid import type"
    }

``405`` Invalid input

/api/import_info ``GET``
-------------------------

List detailed informations about specific imported data.
 
 
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``GET`` http://NODE_IP:PORT/api/import_info?import_id={{import_id}}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "import_id", "true", "file or text", "import_id returned after calling /api/import endpoint"
  
If successful returns detailed informations about desired imported data.


Responses
~~~~~~~~~~~~~~

``200`` Data about desired import sucessfully retrieved.


*Example:*

::

{
    "edges": [],
    "import_hash": "0xe9c407ce686b68bedf758f9513143fbe7c08be12cfc76aa28bf4710e303f4a94",
    "root_hash": "0xea94a2abc75f5e03ff4e5db8adbd2dad26ce9f4d6fa6db6e880891005a715c26",
    "transaction": "0x3711223191991a684ffae3c8672491b1cd74520ca69b91969211d671489f2e71",
    "vertices": []
}

``404`` Bad request. Invalid input parameter (import_id)

*Example:*

::

{
    "message": "Import data for import ID  does not exist"
}

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------------

Replication
============

Replication initiates an offer for a previously imported data set on the blockchain. On success the API route will return the ID of the offer which later can be used to query the status of the created offer. After calling the replication API route, the offer itself will be executing in the background and the node will monitor the offer statuses and bids that other DH nodes are creating as a response to the offer. Please keep in mind that the offer depends on the input parameters setup in the node, which may result in a long bidding time. 

For checking the status of the replication request, see /replication/ :{replication_id} route

/api/replication ``POST``
-----------------------------

Creates an offer and trigger replication.

Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``POST`` http://NODE_IP:PORT/api/replication
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 70

   "import_id", "true", "text", "ID of the import you want to replicate"
   "total_escrow_time_in_minutes", "false", "number", "Total time of the
						       replication in minutes"
   "max_token_amount_per_dh", "false", "text", "Maximum price per
					        DH in token's wei"
   "dh_min_stake_amount", "false", "text", "Minimun stake ammount per
					    DH in token's wei"
   "dh_min_reputation", "false", "number", "Minimum required
					    reputation (0 is the lowest)"
					    
   
   
   
   
This call returns replication_id which can be used in **/api/replication/ :{replication_id}** as input parameter for checking the status of replication.

You can obtain **import_id** as a response from **/api/import** or you can view this value as a vertex attribute in graph database.

*Example:*

::

    {
        "import_id": "0x7e9d30d3f78fd21180c9c075403b1feeace7fbf10e10ad4184dd8b7e38358bc6"
    }

Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
      "replication_id": "60bc3cd1-9b2c-4e12-b59a-14405ec73ce5"
    }

``400`` Import ID not provided

``405`` Failed to start offer


/api/replication/ :{replication_id} ``GET``
--------------------------------------------

Gets the status of the replication with replication_id.

Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``GET`` http://NODE_IP:PORT/api/replication/ {replication_id}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "replication_id", "true", "text", "replication ID for initiated import"
   
Returns one of the statuses for the replication: 

-  **PENDING**: preparation for offer creation (depositing tokens) on the blockchain
-  **STARTED**: replication is initiated and the offer is being written on the blockchain and started waiting for bids
-  **FINALIZING**: the offering time ended and proceeded with choosing bids
-  **FINALIZED**: the offer is finalized on the blockchain and bids choosen
-  **CANCELLED**: the previously created offer was canceled
-  **FAILED**: the offer has failed

You can obtain **replication_id** as a response from call of **POST /api/replication**.

Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
        "offer_status": "PENDING"
    }

``400`` Replication ID is not provided

``404`` Offer not found

-------------------------------------------------------------------------------------------------------------

Read
============

Reading the data from ODN can be performed on 2 ways:

- **Network read** is conducted over ODN in several steps. The node that is executing the call is medium that is conducting read procedure (not the actual data source). This type of read costs TRAC and ETH tokens. Data is acquired from the DH node that provides the bid that meets requirements by the node which requested the data. At the end of network read procedure, newly acquired data is permanently available in local node graph database. Network read has several steps that need to be executed:

1. api/query/network where query is sent across ODN to get data. This call returns QUERYID
2. from /api/query/QUERYID/responses call you can see what are the responses for the query in previous call.
3. /api/read/network with parameters from previous calls will return the data and execute all required token transfers.

- **Local read** from local graph database on the node. The node that is executing the call is the data source. This type of read does not cost any TRAC (or ETH) tokens. It is trusted read from the node that provides the data. That data can be different then data on ODN. Potential difference can be examined by litigation procedure. 

Network
*******


/api/query/network ``POST``
--------------------------------

Publishes a network query for a supply chain data set using simple specific DSL query. The API route will return the ID of the query which can be used for checking the status of the query. The actual quering of the network will last approximately about 1 min, in which period the node will gather the offers for the query responses (read operation) and store them in the internal database storage.

The query must be in JSON format:

::

    { 
        "query": 
            [ 
                {
                    "path": "<SOME_ID>", 
                    "value": "<SOME_VALUE>", 
                    "opcode": "<OPERATOR>" 
                }, ... 
            ] 
    } 
    
Supported operators are: 

-  EQ: when ID equals Value
-  IN: when ID is in Value

Refer to /query/network/{query_param} ``GET``

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/query/network
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query", "true", "DSL query", "DSL query for data on ODN"

*Example:*

::

    {  "query":  [{
    	"path": "identifiers.id",
   	"value": "urn:epc:id:sgln:Building_1",
    	"opcode": "EQ"
    }]
    }

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error or bad request. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. If query was successful additional attribute ‘query_id’ will be present which will contain UUID of the query which can be used to check the result or status of the query.

``400`` Bad request

``500`` Internal error happened on server.


/api/query/{query_id}/responses ``GET``
------------------------------------------

Returns the list of all the offers of the given query. The response will be formatted in an array of JSON objects containing offer details.

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/query/{query_id}/responses
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query_param", "true", "text", "UUID of network query"


Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------



/api/query/network/{query_param} ``GET``
------------------------------------------

Checks the status of the network query

The network query can have the following status:

-  **OPEN**:  the initial status of the query which means it has been published to the network
-  **FINISHED**:  the query has been completed, the required time has elapsed, and the offers must be reviewed via the route ...
-  **PROCESSING**:  the selected offer is currently being processed
-  **FAILED**:  in case of error and a failed query

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/query/network/{query_param}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query_param", "true", "text", "UUID of network query"
   

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------


/api/read/network ``POST``
-------------------------------

Initiates the reading from the network node selected from the previous posted reading offer.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/read/network
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query_id", "true", "text", "ID of the query"
   "reply_id", "true", "text", "ID of the reply"
   "import_id", "true", "text", "ID of the import"
   
*Example:*

::

    {
	 "query_id": "76141d3e-378f-4a9a-8b43-d24f8982ef2e",
	 "reply_id": "fdb5e3ba-9fb0-4a86-910e-110e4b8abd5f",
	 "import_id": "0xe1f05500c1352309e009aaf77f589b4b62b895908da69d7c90ebc5d5c05cf372"
    }
    

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Bad request

*Example:*

::

    {
        "message": "Invalid import type"
    }

-----------------------------------------------------------------------------------------------------------------------

Local read
**********

/api/query/local ``POST``
---------------------------

Run local query on the database

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/query/local
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "query", "true", "text", "DSL query"
   
Returns data from local graph database for requested query.
  
Example

::

    {
        "<IDENTIFIER_KEY1>": "<VALUE1>",
        "<IDENTIFIER_KEY2>": "<VALUE2>", ...
    }


Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``500`` Internal error happened on server

/api/query/local/import ``POST``
--------------------------------------------

Queries a nodes local database and returns all the import IDs that contain the results of the query.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/query/local/import
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "query", "true", "JSON query", "Query object"
   
The query must be in JSON format:

::

    { 
        "query": 
            [ 
                {
                    "path": "<SOME_ID>", 
                    "value": "<SOME_VALUE>", 
                    "opcode": "<OPERATOR>" 
                }, ... 
            ] 
    } 
    
Supported operators are: 

-  EQ: when ID equals Value
-  IN: when ID is in Value




Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``400`` Bad request

``500`` Internal error happened on server



/api/query/local/import:{import_id} ``GET``
------------------------------------------------

Returns given import’s vertices and edges and decrypts them if needed.


.. csv-table:: ``GET`` http://NODE_IP:PORT/api/query/local/import/{import_id}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "import_id", "true", "text", "Import ID attribute"
   
Import ID for example: 0x477eae0227cce0ffaadc235c7946b97cbe2a948fe7782796b53a0c5a6ca6595f

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Param required

-------------------------------------------------------------------------------------------------------------

Local Search
=============

/api/trail ``GET``
-----------------------

Retrieves data on a supply chain product trail from the local database

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/trail
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "queryObject", "true", "text", "Query in specific format ex. vertex_type=BATCH"

Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``500`` Internal error happened on server


/api/fingerprint ``GET``
---------------------------

Gets the fingerprint of a specific import from the blockchain

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/fingerprint
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "dc_wallet", "true", "text", "Data creator’s wallet address"
   "import_id", "true", "text", "Value of import_id from response"

Import_id is received as an response of sucessful /api/import request (Data import ID).

Responses
~~~~~~~~~~~~~~

``200`` Data fingerprint

``400`` Required parameters not provided

``500`` Internal error happened on server


Note: In case of a non existant import_id the returned value will be 0.

-------------------------------------------------------------------------------------------------------------

Profile Token Management
=============

/api/deposit ``POST``
-----------------------

Deposit tokens from wallet to a profile.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/deposit
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query", "true", "JSON query", "Query object"

The query must be in JSON format:

::

    { 
        "atrac_amount": 10
    }

Responses
~~~~~~~~~~~~~~

``200`` Successfully deposited 10 ATRAC to profile

``400`` Bad request

Note: value of 10 used above is just an example, can be any available amount from wallet


/api/withdraw ``POST``
-----------------------

Withdraw tokens from profile to a wallet.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/withdraw
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query", "true", "JSON query", "Query object"

The query must be in JSON format:

::

    { 
        "atrac_amount": 10
    }

Responses
~~~~~~~~~~~~~~

``200`` Successfully withdrawn 10 ATRAC to wallet your_wallet_id

``400`` Bad request

Note: value of 10 used above is just an example, can be any available amount from profile

-------------------------------------------------------------------------------------------------------------

.. _read: http://docs.origintrail.io/en/latest/introduction-to-api.html#read
