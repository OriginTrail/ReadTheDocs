Querying the data
=================

Decentralized Knowledge Graph querying (network query)
------------------------------------------------------

Querying the DKG is done using an array of values that identify a particular object in a dataset.
These identifiers are sent as an array of objects, where path is the type of identifier (such as ean13, sgtin, sgln,
or id for general identifier), value is the identifier value or an array of possible values, and opcode is either EQ or
IN, depending on whether the queried object identifier needs to equal or belong to the given value parameter

::

    {
      "query": [
        {
          "path": "sgtin",
          "value": "urn:epc:id:sgtin:271119.100294475",
          "opcode": "EQ"
        }
      ]
    }



The returned responses contain an array of datasets which contain an object whose identifiers fit the given query.
This response can then be used to import a desired dataset on one's node, which will enable querying the graph locally
or exporting and viewing the dataset.

Local Knowledge Graph querying - Trail
--------------------------------------

Identifier types and values and trail depth
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Querying the local knowledge graph means executing a graph traversal starting from a particular vertex in the graph and traversing over the specified edge types.

The result of the trail represents all objects found on the trail, along with an array that notes which datasets those objects belong to.

{

  "identifier\_types": [

    "ean13"

  ],

  "identifier\_values": [

    "83213023"

  ],

  "depth": 5,

  "connection\_types": [

    "EPC"

  ]

}


identifier\_types and identifier\_values are two arrays used to determine the starting object of the trail. Note that these two arrays must be of the same length, and will be paired in the order they were given (first element of the identifier\_types array corresponds to the first element of the identifier\_values array, etc).

Thedepth parameter determines how far from the starting vertex will the traversal go. If the depth is set to 0 the traversal will return only the objects identified by the given parameters.

Connection types
^^^^^^^^^^^^^^^^

connection\_types is an array which serves as a filter in the graph trail. When observing a vertex in the graph, only the vertices which are connected to the currently observed vertex by a relation type which is in the connection\_types array will be visited and included in the graph.
 Example: vertex\_a is connected to vertex\_b by relation type rel\_type\_1, and vertex\_a is connected to vertex\_c by relation type rel\_type\_2, if the connection\_types includes rel\_type\_1 but not rel\_type\_2, vertex\_b will be included in the trail while vertex\_c will not be

<mozda treba dodati sliku ovde>

In order to avoid backtracking in the trail and attaching superfluous information, a vertex will not be visited if the relation types on the path to that vertex are the same two times in a row.
 Example: vertex\_a is connected to vertex\_b by relation type rel\_type\_1, and vertex\_b is connected to vertex\_c by relation type rel\_type\_1. If connection types array contains rel\_type\_1 then vertex\_b will be included in the trail while vertex\_c will not be, because it is considered that vertex\_c requires backtracking over rel\_type\_1.

If the connection\_types parameter is omitted, the entire graph is traversed (to the specified depth), without the backtracking prevention feature.
