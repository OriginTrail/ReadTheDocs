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

/api/import ``POST``
-------------------------

Import new GS1 or WOT structured data into a node's database. Find out more about data format here :ref:`data-structure-guidelines`
 
 
Parameters
~~~~~~~~~~~~~~
 
.. csv-table:: ``POST`` */api/import
   :header: "Name", "Required", "Type", "Description"
   :widths: 20, 12, 20, 30

   "importfile", "true", "file or text", "data that you want to import (ex. GS1 XML file)"
   "importtype", "true", "predetermined text", "GS1 or WOT. This describes data standard."
  
Importfile ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: **Required** **(Body)** / Import data (file or text data)

*Example:*

::

    {
        "importfile": "importfile=@/ot-node/test/modules/test_xml/GraphExample_4.xml"
    }



Importtype ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Type of provided data (GS1 / WOT)

*Example*

::

    {
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

-------------------------------------------------------------------------------------------------------------------

Replication
============

Replication initiates an offer for a previously imported data set on the blockchain. On success the API route will return the ID of the offer which later can be used to query the status of the created offer. After calling the replication API route, the offer itself will be executing in the background and the node will monitor the offer statuses and bids that other DH nodes are creating as a response to the offer. Please keep in mind that the offer depends on the input parameters setup in the node, which may result in a long bidding time. 

For checking the status of the replication request, see /replication/ :{replication_id} route

/api/replication ``POST``
-----------------------------

Creates an offer and trigger replication

Parameters
~~~~~~~~~~~~~~

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: ID of the import you want to replicate

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


/api/replication/ :{replication_id}
-----------------------------------------

Gets the status of the replication with replication_id.

Parameters
~~~~~~~~~~~~~~

replication_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: ID of the import you have initiated replication for


The status can be one of the following: 

-  PENDING: preparation for offer creation (depositing tokens) on the blockchain
-  STARTED: replication is initiated and the offer is being written on the blockchain and started waiting for bids
-  FINALIZING: the offering time ended and proceeded with choosing bids
-  FINALIZED: the offer is finalized on the blockchain and bids choosen
-  CANCELLED: the previously created offer was canceled
-  FAILED: the offer has failed

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

queryNetwork
=================


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



query ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Query in specific format

*Example:*

::

    "string"

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error or bad request. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. If query was successful additional attribute ‘query_id’ will be present which will contain UUID of the query which can be used to check the result or status of the query.

``400`` Bad request

``500`` Internal error happened on server.



/api/query/{query_id}/responses ``GET``
------------------------------------------

Returns the list of all the offers of the given query. The response will be formatted in an array of JSON objects containing offer details.

query_param ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: UUID of network query.

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------



/api/query/network/{query_param} ``GET``
------------------------------------------

Checks the status of the network query

The network query can have the following status:

-  OPEN:  the initial status of the query which means it has been published to the network
-  FINISHED:  the query has been completed, the required time has elapsed, and the offers must be reviewed via the route ...
-  PROCESSING:  the selected offer is currently being processed
-  FAILED:  in case of error and a failed query

query_param ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: UUID of network query.

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------

Read
============

/api/read/network ``POST``
-------------------------------

Initiates the reading from the network node selected from the previous posted reading offer.

Parameters
~~~~~~~~~~~~~~

query_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

*Example:*

::

    "query_id"

reply_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

*Example:*

::

    "reply_id"

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Bad request

*Example:*

::

    {
        "message": "Invalid import type"
    }

-------------------------------------------------------------------------------------------------------------

Local Search
===============

/api/trail ``GET``
-----------------------

Retrieves data on a supply chain product trail from the local database

Parameters
~~~~~~~~~~~~~~

queryObject ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Query in specific format, for example ?vertex_type=BATCH

Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``500`` Internal error happened on server


/api/fingerprint ``GET``
---------------------------

Gets the fingerprint of a specific import from the blockchain

Parameters
~~~~~~~~~~~~~~

dc_wallet ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Data creator’s wallet address

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Value of import_id received as an response of sucessful /api/import request (Data import ID)



Responses
~~~~~~~~~~~~~~

``200`` Data fingerprint

``400`` Required parameters not provided

``500`` Internal error happened on server


Note: In case of a non existant import_id the returned value will be 0.

-------------------------------------------------------------------------------------------------------------

QueryLocal
============

/api/query/local ``POST``
---------------------------

Run local query on the database

Parameters
~~~~~~~~~~~~~~

query ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

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

Parameters
~~~~~~~~~~~~~~

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

Parameters
~~~~~~~~~~~~~~

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Import ID for example: 0x477eae0227cce0ffaadc235c7946b97cbe2a948fe7782796b53a0c5a6ca6595f

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Param required

