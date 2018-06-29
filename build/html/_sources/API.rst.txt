..  _API:


Import
============

/import ``POST``
-------------------

Import new GS1/WOT data

Find out more about XML structure here :ref:`data-structure-guidelines`

Parameters
~~~~~~~~~~~~~~

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

Creates an offer and trigger replication

/replication ``POST``
-----------------------

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


/replication/ :{replication_id}
-----------------------------------

Asks for replication status

Parameters
~~~~~~~~~~~~~~

replication_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: ID of the import you want to replicate

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

/read/network ``POST``
-----------------------

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

Database
============

/trail ``GET``
-----------------------

Find trail in database

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


/fingerprint ``GET``
-----------------------

Get import fingerprint from blockchain

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

-------------------------------------------------------------------------------------------------------------

QueryLocal
============

/query/local ``POST``
-----------------------
Run local query on graph database

Parameters
~~~~~~~~~~~~~~

query ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Query in specific format

*Example:*

::

    "?uid=urn:epc:id:sgln:HospitalBuilding1"

Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``500`` Internal error happened on server

/query/local/import ``POST``
--------------------------------------------

Parameters
~~~~~~~~~~~~~~

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Import ID for example: 0x477eae0227cce0ffaadc235c7946b97cbe2a948fe7782796b53a0c5a6ca6595f

Responses
~~~~~~~~~~~~~~

``200`` Array of found vertices for given query

``204`` No vertices found

``400`` Bad request

``500`` Internal error happened on server

/query/local/import:{import_id} ``POST``
--------------------------------------------

Parameters
~~~~~~~~~~~~~~

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Import Id

Responses
~~~~~~~~~~~~~~

``200`` OK

``400`` Param required

-------------------------------------------------------------------------------------------------------------

queryNetwork
=================

Network query operations

/query/network ``POST``
--------------------------------

Makes a network query

query ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: Query in specific format

*Example:*

::

    "string"

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. If query was successful additional attribute ‘query_id’ will be present which will contain UUID of the query which can be used to check the result or status of the query.

``400`` Bad request

``500`` Internal error happened on server.

/query/network/{query_param} ``GET``
--------------------------------------

Checks the status of the network query

query_param ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Description: UUID of network query.

Responses
~~~~~~~~~~~~~~

``200`` Always, except on a internal server error. Body will contain message in JSON format containing at least ‘status’ and ‘message’ attribute. ‘message’ will contain the status of the query in format Query status $status.. If status is FINISHED body will contain another attribute ‘vertices’ containing all query result vertices.

``500`` Internal error happened on server.

-------------------------------------------------------------------------------------------------------------

