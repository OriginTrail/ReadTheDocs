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

**Required** **(Body)** / Import data (file or text data)

*Example:*

::

    {
        "importfile": "importfile=@/ot-node/test/modules/test_xml/GraphExample_4.xml"
    }



Importtype ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

Type of provided data (GS1 / WOT)

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

---------------------------------------------------------------------------------------------------------------------------------------

Replication
============

Creates an offer and trigger replication

/replication ``POST``
-----------------------

Parameters
~~~~~~~~~~~~~~

import_id ``required``
^^^^^^^^^^^^^^^^^^^^^^^^^^

ID of the import you want to replicate

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

