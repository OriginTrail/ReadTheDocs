..  _API:


IMPORT
============

/import
-------------------

``POST``

Import new GS1/WOT data

Find out more about XML structure here :ref:`data-structure-guidelines`

Parameters
~~~~~~~~~~~~~~

Importfile
^^^^^^^^^^^^

**Required** **(Body)** / Import data (file or text data)

*Example:*

::

    {
        "importfile": "importfile=@/ot-node/test/modules/test_xml/GraphExample_4.xml"
    }


Responses
~~~~~~~~~~~~~~

``201`` File was successfully imported.


*Example:*

::

    {
        "import_id": "0x477eae0227cce0ffaadc235c7946b97cbe2a948fe7782796b53a0c5a6ca6595f"
    }



EXPORT
============

/export
-------------------

**POST**

Find out more about XML structure here :ref:`data-structure-guidelines`

Parameters
~~~~~~~~~~~~~~
some text


Responses
~~~~~~~~~~~~~~
some text