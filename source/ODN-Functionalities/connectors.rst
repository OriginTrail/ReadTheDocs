Connectors
==========

Connectors are a special type of vertex in the graph that enable traversing data from different data creators within
a single trail request.

When creating a connector it is required to specify a connection identifier and the ERC-725 identity address of the
data creator for whom the connection is intended.

If the connector vertices have matching identifiers, and the connector vertices were created by the corresponding
dataset creators (which is determined by the dataset creator’s identity address), an additional set of two “connector” graph edges is created to enable a cryptographically verifiable connection between the two respective subgraphs.

Note that the connection between two connector vertices will only be created if both data creators specified the
other's ERC-725 node identity in the connector

    **Example:** Data creator Alice replicates a dataset containing a connector CONN1 designated for a data creator Bob.
    When Bob adds a dataset containing a connector CONN1 designated for Alice to the local knowledge graph, a pair of
    edges (one for each direction) with relation type ``CONNECTION_DOWNSTREAM`` will be created between two connector vertices.

For specific information on how to create connectors depending on the data standard, see :ref:`data-structure-guidelines`
