Connectors
==========

Connectors are a special type of vertex in the graph that enable traversing data from different data creators within a
single trail request.

When creating a connector it is required to specify a connection identifier and the ERC-725 identity of the data creator
for whom the connection is intended. An additional graph edge will be created when a connector with the same identifier
which has been created by the specified data creator is added to the graph.

Note that the connection between the connector vertices will only be created if both data creators specified the other's
ERC-725 identity in the connector

Example: Data creator DC1 creates a connector CONN1 for a DC2. When adding a dataset created by DC2 contains a connector
CONN1 for DC1 to the local knowledge graph, an edge with relation type CONNECTION\_DOWNSTREAM will be created between
these two connector vertices.

For specific information on how to create connectors depending on the data standard, see :ref:`data-structure-guidelines`
