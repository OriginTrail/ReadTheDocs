..  _freedom-fourzero:

OT-RFC 01 - OriginTrail v4 Data Layer specification (Freedom - Gemini Release)
================================================================================

Introduction
----------------

This document covers the specification of the OriginTrail v4 data layer
(Freedom-Gemini release). It’s intention is to describe the inner workings of
the OriginTrail Node when it comes to data interoperability, interconnectivity
and integrity features of the Data layer, as well as to attract improvement
proposals and invite contributions from interested developers.

The document is currently under revision.

Technical block scheme
-----------------------

The OriginTrail Data layer is comprised of several components listed below.

.. image:: datalayer1.png
   :width: 600px

*Figure 1.* Data layer structural diagram


Applications access the information on the ODN via an OriginTrail gateway node.
A node can perform local and network queries, resulting in populating it’s local database(s).
The data layer consists of **one or more databases**, multiple **APIs**, versioned OT-JSON **importers** and **converters**.

Vocabulary definition
-----------------------

- **Application (app)**: a software based system that interacts with the data layer via one of the APIs (internal or external). Internal APIs are used by the OT node, while external APIs are used by remote applications
- **API**: Programmable interface that is used to interact with the data layer
- **Blockchain network**: A specific blockchain network that can be identified by name and/or network identifier (i.e. Ethereum Rinkeby testnet, Ethereum mainnet etc)
- **Converter (formerly Transpiler)**: A converter converts a designated dataset format X into OT-JSON file format, and vice versa from OT-JSON to the format X.
- **Cryptographic key pair**: a cryptographically paired set of keys. The Public and Private key pair comprise of two uniquely related cryptographic keys (basically long random numbers).
- **Consensus check**: Operation of comparing value sets from two or more data sources regarding a specific object of mutual interest
- **Database**: any data storage system supporting the use cases of the data layer
- **Dataset**: A standardised file that can be converted to OT-JSON graph format. Example: GS1 EPCIS file
- **Dataset ID**: The system wide identifier for a dataset, which is the result of the cryptographic hashing of the original dataset. At this time, it is equal to the SHA3-256 hash of the dataset.
- **Dataset hash**: The cryptographic hash of the original dataset.
- **Data life span**: The amount of time of how long a dataset replication should be retrievable in minutes
- **Dataset issuer**: A person or system that provides the standardized dataset and introduces is to the node
- **Data standard**: A specified set of rules regarding structuring of the datasets, usually issued by a specific organization such as GS1
- **Data standard ID**: A system wide defined string representing a data standard
- **Data standard schema**: A document schema describing the dataset according to the data standard.
- **Data source**: A system which is able to output datasets in digital format. An example would be a traditional supply chain ERP system, IoT enabled device, cell phone etc.
- **Graph**: A mathematical representation of objects (entities, vertices) and their relations (edges, connections) based on mathematical graph theory
- **Graph elements / entities**: Vertices and Edges in the graph
- **Identifier**: Any string that is specifically designated as an identifier of graph entities
- **Issuer identity**: A decentralized identity associated with cryptographic key pairs belonging to the dataset issuer
- **Merkle proof**: A set of Merkle Tree leaves needed to verify that the Merkle Root hash corresponds to a specific original dataset
- **Merkle Tree**: a tree in which every leaf node is labelled with the hash of a data block, and every non-leaf node is labelled with the cryptographic hash of the labels of its child nodes
- **OT-JSON**: Native ODN data layer file format containing the data represented as a graph, signed by the issuers cryptographic key
- **OT Node**: OriginTrail Network Node, a unit of the OriginTrail Network
- **OriginTrail Network**: A network consisting of OT Nodes
- **Root hash**: The root node cryptographic hash of the Merkle tree produced from the dataset

Architecture proposal
----------------------

The V4 data layer architecture is a multi level architecture, following ANSI-SPARC architecture guidelines for separation of levels.

.. image:: architecture-img.png
   :width: 400px

*Figure 1.* ANSI-SPARC architecture

On the top level of OT data layer is the external level, defined by external standards, such as GS1, WOT, etc. Files from the external level are converted to the conceptual level, which uses the OT-JSON structure for representation of files. OT-JSON represents the main data structure in OT data layer. To store the OT-JSON document, it is required for the document to be first converted to the internal level, separating the dataset graph data and dataset metadata into separate JSON structures. Finally, the internal level is converted to the  database level which stores all the data in the database(s). It is important that all conversion operations are reversible, so that the data from the  database level can be restored back to external level without loss of information.

.. image:: datalayer3.png
   :width: 400px

*Figure 2.* OT Datalayer architecture


Data layer acceptance criteria
--------------------------------

Data layer requirements & User stories
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

General use case related requirements

**Requirement id**: R1
    - **Requirement name**: Storing / reading arbitrary data in a graph
    - **Requirement story**: As an app I can store arbitrary data in a graph and retrieve it back.
    - **Requirement type**: Boolean (True)

**Requirement id**: R2
    - **Requirement name**: Identifying use case entities/events by one or multiple identifiers
    - **Requirement story**: As an app I can identify graph entities by one or more identifiers.
    - **Requirement type**: Boolean (True)


**Requirement id**: R3
    - **Requirement name**: Returning all data related to a specific identifier according to specific filters on a graph level (i.e. all business locations that have been involved with a certain truck with an ID)
    - **Requirement story**: As an application I can fetch all graph data related to an identifier X according to available filters.
    - **Requirement type**: Boolean (True)


**Requirement id**: R4
    - **Requirement name**: Creating relations between entities and files/documents
    - **Requirement story**: As an app I can connect different graph entities through the internal API.
    - **Requirement type**: Boolean (True)


**Requirement id**: R5
    - **Requirement name**: Verification/Provenance of data integrity
    - **Requirement story**: As an app I can find a corresponding fingerprint on a blockchain, as well as retrieve a Merkle proof related to part of the dataset that was returned from a traversal query.
    - **Requirement type**: Boolean (True)


**Requirement id**: R6
    - **Requirement name**: Determine/Confirm data provider from the data itself
    - **Requirement story**: As an app I can identify the issuer of each graph element returned by the API.
    - **Requirement type**: Boolean (True)


**Requirement id**: R7
    - **Requirement name**: Invalidating existing datasets
    - **Requirement story**: As an app I can publish a dataset that will contain information about invalidating a previously published dataset. This dataset can contain new information previously unpublished
    - **Requirement type**: Boolean (True)


**Requirement id**: R8
    - **Requirement name**: Traversing the data with specific criteria
    - **Requirement story**: As an app I can perform the graph traversal based on a specific identifier with specific criteria (such as graph depth, connection type etc)
    - **Requirement type**: Boolean (True)


**Requirement id**: R9
    - **Requirement name**: Support ZeroKnowledge applications for quantity balances
    - **Requirement story**: As an app I can perform zero-knowledge calculations for quantity balance based on the data obtained from the graph traversal operation
    - **Requirement type**: Boolean (True)


**Requirement id**: R10
    - **Requirement name**: Support ZeroKnowledge applications for set inclusion
    - **Requirement story**: As an app I can determine if a certain piece of data is part of a zero knowledge set
    - **Requirement type**: Future


**Requirement id**: R11
    - **Requirement name**: Support future ZeroKnowledge applications
    - **Requirement story**: As an app I can expect to support future zero knowledge applications on graphs, as I can store and query them.
    - **Requirement type**: Boolean (True)


**Requirement id**: R12
    - **Requirement name**: Providing data for consensus checks across multiple dataset connections (events)events (was single event so far)
    - **Requirement story**: As an app I can determine if there was a match between two different graphs, coming from two different datasets by two different issuers which have indicated each others identities.
    - **Requirement type**: Future


**Requirement id**: R13
    - **Requirement name**: Connecting data of different data providers
    - **Requirement story**: As a node I am can connect datasets from different data issuers based on specific identifiers.
    - **Requirement type**: Boolean (True)


**Requirement id**: R14
    - **Requirement name**: Perform network queries
    - **Requirement story**: As a node I should be able to query the whole network and store the results in my local database.
    - **Requirement type**: Boolean (True)




Standards related
~~~~~~~~~~~~~~~~~~~~~

**Requirement id**: R15
    - **Requirement name**: Mapping a large number of external standards (GS1-EPCIS, WOT, DID, …) to OT-JSON format
    - **Requirement story**: As a data issuer I should be able to import any type of standardized dataset to my node as long as there is a corresponding converter created for this dataset.
    - **Requirement type**: Boolean (True)

**Requirement id**: R16
    - **Requirement name**: 1-1 backward conversion of OT-JSON to source external format
    - **Requirement story**: As a converter I can convert my designated input file format X to OT-JSON and convert the resulting OT-JSON format back to format X without any data loss.
    - **Requirement type**: Boolean (True)

**Requirement id**: R17
    - **Requirement name**: Enabling GS1 EPCIS Query mechanism
    - **Requirement story**: As an app I can use the GS1 EPCIS query mechanism API to access data on the OT node.
    - **Requirement type**: Future


Architecture related
~~~~~~~~~~~~~~~~~~~~~

**Requirement id**: R18
    - **Requirement name**: Usage of multiple databases (Database agnosticism)
    - **Requirement story**: As a developer I can utilize multiple databases in the OriginTrail data layer, provided that an appropriate database interface is created.
    - **Requirement type**: Range (> 3)

**Requirement id**: R19
    - **Requirement name**: Converter - OT-JSON decoupled architecture
    - **Requirement story**: As a converter I am not in any way dependent on the implementation of the OT-JSON importer, and vice versa.
    - **Requirement type**: Boolean (True)

**Requirement id**: R20
    - **Requirement name**: Non-blocking API for importing
    - **Requirement story**: As an API, I should not be blocked during the operation of import, whatever the import operation run time.
    - **Requirement type**: Boolean (True)


Performance related requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


**Requirement id**: R21
    - **Requirement name**: Importing large datasets
    - **Requirement story**: As a user I should be able to import datasets of arbitrary large size. The implementation of the data layer should not provide constraints on the file size. Other limitations might exist stemming from the operating system, hardware limits or other protocol limitations.
    - **Requirement type**: Range (> 1GB)

**Requirement id**: R22
    - **Requirement name**: Challenge block response in constant time
    - **Requirement story**: As a node I should be able to challenge other nodes in short (constant) time.
    - **Requirement type**: Boolean (True)


OT-JSON essentials
---------------------

    - **Objects** - Use case entities (products, locations, vehicles, people, … )
    - **Relations** - Relations between use case entities (INSTANCE_OF, BELONGS_TO, … )
    - **Metadata** - Data about dataset (integrity hashes, data creator, signature, transpilation data, ....)

**Example**

Assuming that use case request is to connect products with factories there they are produced. Entities of the use case are Product and Producer.
 These entities are represented as **objects** in OT-JSON format. Product can have **relation** PRODUCED_BY with producer that produces it and the producer
 can have **relation** HAS_PRODUCED with the product. Product and producer have unique identifiers Product1, Producer1 respectively.

.. image:: datalayer4.png

*Figure 2.* Diagram of the example entities and relations


.. code:: json

    @graph: [
        {
            “@id”: “Product1”,
            “@type”: “OTObject”,
            “identifiers”: [
                {
                    “identifierType”: “ean13”,
                    “identifierValue”: “0123456789123”,
                }
            ],
            “properties”: {
               “name”: “Product 1”
               “quantity”: {
                   “value”: “0.5”,
                   “unit”: “l”
                }
            },
            “relations”: [
                {
                    “@type”: “OTRelation”,
                    "linkedObject": {
                            "@id": "Producer1"
                        },
                    "properties": {
                            "relationType": "PRODUCED_BY"
                        }
                }
            ]
        },
        {
            “@id”: “Producer1”,
            “@type”: “OTObject”,
            “identifiers”: [
                {
                    “identifierType”: “sgln”,
                    “identifierValue”: “0123456789123”,
                }
            ],
            “properties”: {
               “name”: “Factory 1”
               “geolocation”: {
                   “lat”: “44.123213”,
                   “lon”: “20.489383”
                }
            },
            “relations”: [
                {
                    “@type”: “OTRelation”,
                    "linkedObject": {
                            "@id": "Product1"
                        },
                    "properties": {
                            "relationType": "HAS_PRODUCED"
                        }
                }
            ]
        }
    ]

*Figure 3.* OT-JSON graph representing example entities

Conceptual essentials
------------------------

Here are some essential conceptual things related to the data in a dataset.
Try to fit example of book as an object from the physical world with its information as the data.

    - Every OT-JSON entity (Object) is identified with at least one unique identifier. An identifier is represented as a non-empty string.
    - Entities can have multiple identifiers along with the unique one. For example: EAN13, LOT number and time of some event.
    - Data can be connected by arbitrary relations. A user can define own relations that can be used with others defined by standard.
    - Relations are directed from one entity to another. It is possible to create multiple relations between two objects in both directions.


OT-RFC-02 - Data Layer v4 API
===============================

Introduction
----------------

The upcoming version of the OriginTrail node (v4 - Freedom-Gemini release) will entail a new version of the protocol data layer with an updated API based on the usage experience of the ODN mainnet. The purpose of this document is to present the design of the next version of the node Data layer API in detail and attract community improvement proposals, feedback and any additional productive ideas that might positively influence the design of the protocol and solutions built using it.

The release of the v4 node is scheduled for Q4 2019.

Important note on compatibility
---------------------------------

The proposed solution introduces several breaking changes while aiming to solve current API limitations. Import with automatic replication is not supported anymore. Furthermore import, replication and export API routes are designed to be non-blocking which changes the way they are utilized.


Solution proposition
-----------------------

**API Handlers**

To eliminate blocking calls and avoid long wait times for API calls, handlers are returned when requesting operations which take time to process. The handler value is generated as a UUID v4 and used throughout the operation as the request ID.
This value later can be used in queries to retrieve the status of the operation along with the appropriate data.
A global expiration time should be defined for handlers, after this period both the handler and the result will be deleted. This expiration time should be a customizable field in a node’s configuration.

**API Reference and call explanation**

**Import**

When a user wishes to import a specific dataset into the node database by using the import API route, the node will transform the input dataset into graph form and store it into its graph database. The import request will generate a handler that is used later to query the import status. The queried result will return a data field with relevant information about the import, along with the status field.
If an import fails to complete for any reason, the data field will contain the error message and the status field will marked as FAILED.


OT-RFC-03: OriginTrail v4 Data Layer - OT-JSON v1.0 data structure
=====================================================================


OT-RFC-04: OriginTrail Data Layer - Graph data structure (internal level)
===========================================================================


OT-RFC-05: OriginTrail v4 Data Layer - Database Level Structure
==================================================================
