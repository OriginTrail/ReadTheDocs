..  _data-layer:

Data Layer
======================================

The purpose of this document is to explain the data structures of the
OriginTrail protocol data layer. OriginTrail is a purpose built protocol
for trusted supply chain data sharing, utilizing GS1 standards and based
on blockchain.

The following abstractions are based on the observations from several
years of working with supply chain transparency solutions. The aim of
the abstraction is to be generic enough to support any present and
future use cases involving supply chain event visibility and data
exchange, while on the other hand being as tailored as possible to
provide optimal technical performance to support such cases.

Underlying technology and technical rationale
---------------------------------------------

The OriginTrail Decentralized Network (ODN) Data Layer is intended to
provide data interoperability (between different data providers - i.e.
supply chain stakeholders), as well as easy interconnectivity between
different data sets regardless of their source. This suggests that the
very nature of data structures in play is as much about the information
itself, as it is about the connections between the data provided from
different sources. Network structure consists of incentivised nodes (:ref:`incentive-model`).

During the years of building supply chain transparency solutions we have
come to the conclusion that the most adequate data structure is a graph,
where the connections between data points are “first-class citizens” of
the structure. Our previous implementations involved relational
databases which have proven to be suboptimal, finally converging towards
graph logic emulation within the table structures. A native graph
database has shown to be superior in terms of the problems OriginTrail
is tackling.

A graph is ideal to represent the chain of custody on every trackable
element in the supply chain, its properties over time and its
interactions with facilities, transportation devices, companies and
people. The structure of supply chain data in the graphs of the data
layer is subject of this document and is based on GS1 standards, but not
exclusive to other standardization schemes and aims to be standard
inclusive.

It is important to note that these graph abstractions are not dependent
on the specific implementation of the underlying graph database. The
data layer is intended to be “plug-and-play” in this regard, allowing
the choice of the underlying database as long as it can support the
structures and features needed by the data layer. Introducing data to
the Data layer. In this way the system can be extended to support future data formats
and providers.

Once the data gets converted into graph form and stored in the database,
its fingerprint is stored on the blockchain. This process, as well as
the specific details on importer implementations, are out of the scope
of this document, but will be subject of further documentation.

Entities in graph structure (ontology)
--------------------------------------

There are 5 generic entity groups in the structure of OriginTrail graph
structure.

-  **Objects** and **ObjectClasses**, represented as nodes
-  **Events** and **EventClasses**, represented as nodes
-  **Connections**, represented as edges

Each entity can contain

1. An **identifier** block The identifier block contains a set of IDs
   describing the particular entity. Examples would include identifiers
   such as GS1 codes (i.e. EAN8, EAN13), QR codes, batch identifiers
   (LOT or serial numbers) or any type of internal identifiers used by
   the parties in the supply chain. These allow for easy lookups and
   traversals. Additionally each graph element has a unique ID generated
   deterministically by the system.
2. A **data** block: The data block contains all non-ID related
   information about the entity and is extensible.
3. A **meta data** block This meta data involves information about the
   import process, signing keys and other technical details, not related
   to the meta data about supply chain products.

Objects and ObjectClasses
~~~~~~~~~~~~~~~~~~~~~~~~~

Objects represent all physical or digital entities involved in events in
the supply chain. Examples of objects are vehicles, production
facilities, documents, sensors, personnel etc. ObjectClasses
specifically define a global set of properties for their child Objects
(as their “instances”). In the example of a wine authenticity use case,
the data shared among supply chain entities (winery, distributors,
retailers etc) involves information about specific batches of bottles
with unique characteristics. The master data about a product would
present an ObjectClass node in the OT graph, while the specifics about
the product batch would be contained within the “batch” Object. This
allows for a hierarchical organization of objects, with a simplistic but
robust class-like inheritance.

ObjectClasses are divided in:

-  **Actors**, which encompass companies, people, machines and any other
   entity that can act or observe objects within the supply chain. (the
   “Who”)
-  **Products** (supply chain objects of interest), which represent
   goods or services that are acted upon by actors (the “What”)
-  **Locations**, which define either physical or digital locations of
   products or actors (the “Where”)

Each of the Objects can then be further explained by custom defined
subcategories.

Events and EventClasses
~~~~~~~~~~~~~~~~~~~~~~~

Events in the graph structure have a similar inheritance pattern –
EventClasses classify types of events which are instantiated as
particular Event nodes. OriginTrail currently classifies 4 different
event types:

-  **Transport** events, which explain the physical or digital
   relocation of objects in the supply chain.
-  **Transformation** events, which contain information about the
   transformation of one or more objects into (a new) one. An example
   would be the case of an electronic device (i.e. mobile phone), where
   the assembly is observed as a transformation event of combining
   different components – Objects - into one output Object, or the case
   of combining a set of SKUs in one group entity such as a
   transportation pallet. Similarly, a digital transformation event
   would be any type of processing of a digital product (i.e. mastering
   of a digital sound recording). This event type corresponds to GS1
   AggregationEvents and TransformationEvents.
-  **Observation** events, which entail any type of observational
   activity such as temperature tracking via sensors or laboratory
   tests. This event corresponds to GS1 ObjectEvents that are published
   by one party (interaction between different business entities is not
   the primary focus of the event).
-  **Ownership/custody transfer** events, where the change of ownership
   or custody of Objects is distinctly explained. An example would be a
   sale event.

Each of the events can then be further explained by custom defined
subcategories and meta data.

Connections
~~~~~~~~~~~

The connections are edges in the graph used to define connections
between Objects, ObjectClasses and Events. The connections are
classified in 3 groups:

-  **Inheritance** connections (between ObjectClass and Object vertices,
   as well as between EventClass and Event vertices). These connections
   define that an Object is an instance of ObjectClass, the isInstanceOf
   edge.
-  **Involvement** connections (between Object and Event vertices)
   connect objects with events in which they are involved. For example,
   a transformation event of production would have input objects, output
   objects, a location where the production took place etc.
-  **State** connections (between two Object vertices) connect two or
   more objects that are related in some way. For example, an object can
   be owned by some supply chain actor.

An example of a graph illustrating all the above mentioned entities is
provided below. It illustrates a simplified scenario of a movement of an
“engine” object within a supply chain, undergoing a transformation event
of wrapping it up in a “package” by Employee Bob from CarEngines LTD,
which is transported by an “AirTransport” company and finally sold to
“Buyer Alice Corp”.

|Graph Example|

This very simple example easily illustrates the amount of data
connections needed to efficiently explain the undergoing scenario and is
simplified to provide an easier logical overview.

Conclusion and future steps
---------------------------

This document outlines the data structure logic behind OriginTrail’s
data layer with the intention of providing high flexibility and data
interoperability within the network. It is a work in progress and
therefore we invite the community to join and provide ideas and feedback
on possible improvements and inefficiencies that may arise from such a
scheme. The best way to contribute is to use the Improvement Proposals
repository provided by the OriginTrail team. Further iterations on the
structure will be based on use-cases implemented and observed in the
alpha and test net focusing on optimizations and simplifications of the
structure.

.. |Graph Example| image:: http://pulsing-ks.com/tmp/graph-example.png