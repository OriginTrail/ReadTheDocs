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

1. **/api/import** with data as parameter is importing the data from data source to local graph database on DC node. data_set_id is returned to data creator as a parameter in response.
2. **/api/replication/** with data_set_id as input parameter is replicating the data from local graph database on DC node to other DH nodes via bidding mechanism.

After all steps are completed, imported data can be read from ODN via `read`_ procedure.

/api/import ``POST``
-------------------------

Import new GS1 or WOT structured data into a node's database.
Required POST body parameters for import are importtype (GS1/WOT), and importfile (XML/JSON data). 
Import can be also done by uploading the file containing data using multipart form. In that case, name of the field containing data file should also be named importfile. 
If automatic replication after import is required, parameter replicate with value set to true can be provided. 
Find out more about data format here :ref:`data-structure-guidelines` 
 
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``POST`` http://NODE_IP:PORT/api/import
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "importfile", "true", "file or text", "data that you want to import (ex. GS1 XML file)"
   "importtype", "true", "predetermined text", "GS1 or WOT. This describes data standard."
   "replicate", "false", "boolean", "Trigger automatic replication phase for this dataset"
  
If successful returns data_set_id for this data import.

*Example:*

::

    {
        "importfile": "importfile=@/ot-node/importers/xml_examples/Basic/01_Green_to_pink_shipment.xml"
        "importtype": "GS1"
    }

Responses
~~~~~~~~~~~~~~

``201`` File was successfully imported.


*Example:*

::

    {
        "data_set_id": "0xf0a61417b29e042c6b5591e5db55a8f4785558a5c76efa474a8d6464aa19ccbb",
        "message": "Import success",
        "wallet": "0x39591394670eF0A062DE2551EE58e584e8732c01"
    }

``400`` Invalid import parameters (importfile/importtype)

*Example:*

::

    {
         "message": "Invalid import type"
    }

``405`` Invalid input

/api/import_info ``GET``
----------------------------

Fetch import information for specific data-set id. Query param is data-set id.
Response contains details of requested data-set.
 
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``GET`` http://NODE_IP:PORT/api/import_info?data_set_id={{data_set_id}}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "data_set_id", "true", "text", "data set id returned after calling /api/import endpoint"
  
If successful returns detailed informations about desired imported data.


Responses
~~~~~~~~~~~~~~

``200`` Data about desired import sucessfully retrieved.


*Example:*

::

    {
    	"import" : {
            "edges": [...],
            "vertices": [...]    
        },
    	"data_provider_wallet": "0x39591394670eF0A062DE2551EE58e584e8732c01",
    	"root_hash": "0xe6db4945d722e132db2abe74f5aa0991f232fa471dce587feef36786b6e915a2",
    	"transaction": "null"
    }

``404`` Bad request. Invalid input parameter (data_set_id)

*Example:*

::

    {
    	"message": "Import data for data set ID 0xf0a61417b29e042c6b5591e5db55a8f4785558a5c76efa474a8d6464aa19ccbb does not exist"
    }

``500`` Failed to get information about imports.

/api/imports_info ``GET``
----------------------------

List information for all imported datasets. No query parameters required. Response contains list of information about imported datasets.
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``GET`` http://NODE_IP:PORT/api/imports_info
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "none", "", "", ""


Responses
~~~~~~~~~~~~~~

``200`` Data about desired imports sucessfully retrieved.


*Example:*

::

    {
        "data_provider_wallet": "0x39591394670eF0A062DE2551EE58e584e8732c01",
        "data_set_id": "0xaac45a9ffae8620d98e4cf209d3b250e191835a69b25ff4e4c1c539c5f972984",
        "data_size": 4404,
        "root_hash": "0xd4363f9f118e9f0a60663801da0bcaf39b2b0a57da23258400df0e5c292d5ee1",
        "total_documents": 35,
        "transaction_hash": null
    },
    {
        "data_provider_wallet": "0x39591394670eF0A062DE2551EE58e584e8732c01",
        "data_set_id": "0xe97c9c934b0ee5cb443f529aa3174ac462619b59855649b16ef071f884afabf9",
        "data_size": 4404,
        "root_hash": "0xc2455c18c815033dcae3e70ff1b4bd35c951802ed5357d79c5df2ff783075f59",
        "total_documents": 35,
        "transaction_hash": "0x93dcb8bb3210e50e01796873c64671d9f6c033548f4e0008cdc1b8bb151f87f2"
    },
    {
        "data_provider_wallet": "0x39591394670eF0A062DE2551EE58e584e8732c01",
        "data_set_id": "0xf0a61417b29e042c6b5591e5db55a8f4785558a5c76efa474a8d6464aa19ccbb",
        "data_size": 7241,
        "root_hash": "0xe6db4945d722e132db2abe74f5aa0991f232fa471dce587feef36786b6e915a2",
        "total_documents": 86,
        "transaction_hash": null
    }

``500`` Failed to get information about imports.

-------------------------------------------------------------------------------------------------------------------

Replication
============

Replication initiates an offer for a previously imported data set on the blockchain. On success the API route will return the ID of the offer which later can be used to query the status of the created offer. After calling the replication API route, the offer itself will be executing in the background and the node will monitor the offer statuses and bids that other DH nodes are creating as a response to the offer. Please keep in mind that the offer depends on the input parameters setup in the node, which may result in a long bidding time. 

For checking the status of the replication request, see /replication/:{replication_id} route

/api/replication ``POST``
-----------------------------

Creates an offer and trigger replication.

Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``POST`` http://NODE_IP:PORT/api/replication
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 70

   "data_set_id", "true", "text", "Data set id of the import you want to replicate"
   "holding_time_in_minutes", "false", "number", "Total time of the replication in minutes"
   "token_amount_per_holder", "false", "text", "Token amount to be paid to each DH in token's 10^-18 TRAC"
   "litigation_interval_in_minutes", "false", "text", "Minimum time between two litigation requests concerning one DH"

   
   
This call returns replication_id which can be used in **/api/replication/:{replication_id}** as input parameter for checking the status of replication.

You can obtain **data_set_id** as a response from **/api/import** or you can view this value as a vertex attribute in graph database.

*Example:*

::

    {
        "data_set_id": "0xf0a61417b29e042c6b5591e5db55a8f4785558a5c76efa474a8d6464aa19ccbb"
    }

Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
      "replication_id": "0afc2a18-87cb-44de-b1e9-61694d5ea5f6",
      "data_set_id": "0xe97c9c934b0ee5cb443f529aa3174ac462619b59855649b16ef071f884afabf9" 
    }

``400`` Invalid parameters

``404`` This data set does not exist in the database


/api/replication/:{replication_id} ``GET``
--------------------------------------------

Gets the status of the replication with replication_id.

Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``GET`` http://NODE_IP:PORT/api/replication/{replication_id}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "replication_id", "true", "text", "replication ID for initiated import"
   
Returns one of the statuses for the replication: 

-  **STARTED**: Replication is initiated, offer has been written on the blockchain and started waiting for bids
-  **PREPARED**: Preparation for offer creation (depositing tokens) on the blockchain
-  **PUBLISHED**: Published to blockchain
-  **MINED**: Found a solution for DHs provided
-  **CHOOSING**: The offering time ended and proceeded with choosing bids
-  **FINALIZED**: The offer is finalized on the blockchain and bids choosen
-  **CANCELLED**: The previously created offer was canceled
-  **FAILED**: The offer has failed

Note: miner is not the same as miner in the context of blockchain. That's how we choose identities for finalizing the offer.

You can obtain **replication_id** as a response from call of **POST /api/replication**.

Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
        "message": "Offer has been successfully started. Waiting for DHs...",
        "offer_id": "0xf0309fce477cdceca04f372e58365725d460c3c7800a72aa37b20f1c6b7d5383",
        "status": "STARTED"
    }

``400`` Replication ID is not provided

``404`` Offer not found

-------------------------------------------------------------------------------------------------------------

Network Read
============

Reading the data from ODN can be performed on 2 ways:

- **Network read** is conducted over ODN in several steps. The node that is executing the call is medium that is conducting read procedure (not the actual data source). This type of read costs TRAC and ETH tokens. Data is acquired from the DH node that provides the bid that meets requirements by the node which requested the data. At the end of network read procedure, newly acquired data is permanently available in local node graph database. Network read has several steps that need to be executed:

1. api/query/network where query is sent across ODN to get data. This call returns QUERYID
2. from /api/query/QUERYID/responses call you can see what are the responses for the query in previous call.
3. /api/read/network with parameters from previous calls will return the data and execute all required token transfers.

- **Local read** from local graph database on the node. The node that is executing the call is the data source. This type of read does not cost any TRAC (or ETH) tokens. It is trusted read from the node that provides the data. That data can be different then data on ODN. Potential difference can be examined by litigation procedure. 


/api/query/network ``POST``
--------------------------------

Publishes a network query for a supply chain data set using simple specific DSL query. The API route will return the ID of the query which can be used for checking the status of the query. 
The actual quering of the network will last approximately about 1 min, in which period the node will gather the offers for the query responses (read operation) and store them in the internal database storage.

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

    { 
        "query": 
            [ 
                {
                    "path": "identifiers.id", 
                    "value": "urn:epc:id:sgln:Building_1", 
                    "opcode": "EQ" 
                }, ... 
            ] 
    }

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error or bad request. Body will contain message in JSON format containing at least ‘message’ attribute. If query was successful additional attribute ‘query_id’ will be present which will contain UUID of the query which can be used to check the result or status of the query.

*Example:*

::

    {
        "message": "Query sent successfully.",
        "query_id": "12e99b1e-ef25-4f51-9372-ec4186d1d1b6"
    }

``400`` Bad request

``500`` Internal error happened on server.


/api/query/{query_id}/responses ``GET``
------------------------------------------

Get currently received responses for given query. The response will be formatted in an array of JSON objects containing offer details (reply id, dataset id, price and size of dataset).

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/query/{query_id}/responses
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query_param", "true", "text", "UUID of network query"


Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

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

Initiate network read for selected response. Parameters provided in the body are query id. dataset id and reply id structured in JSON format.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/read/network
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "query_id", "true", "text", "ID of the query"
   "reply_id", "true", "text", "ID of the reply"
   "data_set_id", "true", "text", "ID of the imported data set"
   
*Example:*

::

    {
	 "query_id": "76141d3e-378f-4a9a-8b43-d24f8982ef2e",
	 "reply_id": "fdb5e3ba-9fb0-4a86-910e-110e4b8abd5f",
	 "data_set_id": "0xe1f05500c1352309e009aaf77f589b4b62b895908da69d7c90ebc5d5c05cf372"
    }
    

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Bad request

-----------------------------------------------------------------------------------------------------------------------

Local Read
============

/api/query/local ``POST``
---------------------------

Querying data from local database. Get list of identifiers of datasets containing vertices that match given query. 
Data location query is submitted in JSON form through POST body. Query represents list of objects containing query parameters path (name of queried field), value (value of queried field), opcode (operator representing relation of field value and queried value - EQ/IN).
Response contains list of dataset identifiers.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/query/local
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "query", "true", "text", "DSL query"
   
Returns data from local graph database for requested query. 
The query must be in JSON format:

::

    { 
        "query": 
            [ 
                {
                    "path": "<SOME_ID>", 
                    "value": "<SOME_VALUE>", 
                    "opcode": "<OPERATOR>" 
                }
            ] 
    } 
    
Supported operators are: 

-  EQ: when ID equals Value
-  IN: when ID is in Value

Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``500`` Internal error happened on server


/api/query/local/import:{data_set_id} ``GET``
------------------------------------------------

Get raw dataset data. Response contains dataset vertices and edges.


.. csv-table:: ``GET`` http://NODE_IP:PORT/api/query/local/import/{data_set_id}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30
   
   "data_set_id", "true", "text", "Import ID attribute"
   
*Example:*

::

    {
        "data_set_id": "0xf0a61417b29e042c6b5591e5db55a8f4785558a5c76efa474a8d6464aa19ccbb"
    }

Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
        "edges": [...],
        "vertices": [...]
    }

``400`` Bad request, param required

-------------------------------------------------------------------------------------------------------------

Local Search
=============

/api/trail ``GET``
-----------------------

Retrieves data on a supply chain product trail from the local database

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/trail
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "queryObject", "true", "text", "Query in specific format ex. identifiers.uid=urn:epc:id:sgtin:Batch_1"

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
   
   "data_set_id", "true", "text", "Value of the imported data set id"

data_set_id is received as an response of sucessful /api/import request (Data set import ID).

Responses
~~~~~~~~~~~~~~

``200`` Data fingerprint

``400`` Required parameters not provided

``500`` Internal error happened on server


Note: In case of a non existant data_set_id the returned value will be 0.

-------------------------------------------------------------------------------------------------------------

Profile Token Management
============================

/api/balance ``GET``
-----------------------

Provides information about available funds on node's profile and wallet.

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/balance
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "humanReadable", "false", "boolean", "When set to true, response gets shown in human friendly format."


Responses
~~~~~~~~~~~~~~

``200`` Successfully retrieved info about node's profile and wallet funds

*Example:*

::
 
    {
        "profile": {
            "minimalStake": "1000",
            "reserved": "0",
            "staked": "1000"
        },
        "wallet": {
            "address": "0x39591394670eF0A062DE2551EE58e584e8732c01",
            "ethBalance": "99.627180745",
            "tokenBalance": "4999999999000"
        }
    }

``503`` Failed to get balance.


-------------------------------------------------------------------------------------------------------------

Consensus check
============================

/api/consensus/{sender_id} ``GET``
------------------------------------

Returns list of OwnershipTransfer type events of the given data sender from database. If the event is connected with another event of the same type from different sender, response contains events from both sides.

.. csv-table:: ``POST`` http://NODE_IP:PORT/api/consensus/{sender_id}
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "sender_id", "true", "text", "Identifier of sender, for example urn:ot:object:actor:id:Company_Pink"


Responses
~~~~~~~~~~~~~~

``200`` OK

*Example:*

::

    {
        "events": [
        {   
            "side1": {...}
        },
        {
            "side1": {...}
        },
        {
            "side1": {...},
            "side2": {...}
        }
        ]
    }

``400`` Bad request


-------------------------------------------------------------------------------------------------------------


Node information
======================

/api/info ``GET``
----------------------------

Returns basic configuration information about running node.

Parameters
~~~~~~~~~~~~~~

.. csv-table:: ``GET`` http://NODE_IP:PORT/api/info
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "none", "", "", ""


Responses
~~~~~~~~~~~~~~

``200`` Basic node configuration data sucessfully retrieved.


*Example:*

::

    {
        "blockchain": "Ethereum",
        "erc_725_identity": "0x87d31a77ec3bd2fd162a6eead13d744dba0103a6",
        "is_bootstrap": false,
        "network": {
            "contact": {
                "agent": "1.0.0",
                "hostname": "95.91.215.153",
                "index": 8885,
                "network_id": "TestnetV2.0.1b",
                "port": 5278,
                "protocol": "https:",
                "wallet": "0x0556B6d87f011424C7b099040f03AD23774FE7FE",
                "xpub": "xpub661MyMwAqRbcFWt6qoWR1oYKwAnqYW63PJWKAJuBFt9ipgt8sYXNkuHqHi1GLWdPfw9eS2TLw8rTrVQsPV6P4LqZPm8CCxJGC6AaycKA3Sv"
            },
            "identity": "b93b0cc6fc4cb65a9f921ca909597f74ab92d59b"
        },
        "node_wallet": "0x0556B6d87f011424C7b099040f03AD23774FE7FE",
        "version": "2.0.29"
    }

``500`` Failed to get information about basic node configuration.

-------------------------------------------------------------------------------------------------------------------

.. _read: http://docs.origintrail.io/en/latest/introduction-to-api.html#read
