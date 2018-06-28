..  _namespace:

Namespace
======================================================

The EPCIS standard places general constraints on the identifiers that
End Users may create for use as User Vocabulary elements. Specifically,
an identifier must conform to URI syntax, and must either conform to
syntax specified in GS1 standards or must belong to a subspace of URI
identifiers that is under the control of the end user who assigns them.

**OriginTrail namespaces are mostly used for attributes definitions in
EPCIS based XML file. Values for that attributes should be based
on**\ `GS1 identifiers`_\ **if it is possible.**

For more information check:

-  GS1 `CBV document`_
-  GS1 `EPCIS standard`_

For more information about data structuring in OriginTrail check
`guidelines`_

GS1 CBV Namespace
--------------------

The Core Business Vocabulary provides additional constraints on the
syntax of identifiers for user vocabularies, so that CBV-Compliant
documents will use identifiers that have a predictable structure. This
in turn makes it easier for trading partners to understand the meaning
of such identifiers. **GS1 Namespace should be used always before
OriginTrail namespace if it is possible.**

OriginTrail Namespace
------------------------

OriginTrail created it's own namespace as extension of GS1 namespace.
Root of the name space is **urn:ot:**. We strongly advise use of our
namespace as described below. OriginTrail protocol name space is based
on `data model`_. If OriginTrail namespace is not used properly it can
cause deviations in data structure. We strongly recommend data
validation in Graph database after first iterations.

Object urn:ot:object
--------------------

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

Actor urn:ot:object:actor
~~~~~~~~~~~~~~~~~~~~~~~~~

Actors, which encompass companies, people, machines and any other entity
that can act or observe objects within the supply chain. (the “Who”).

urn:ot:object:actor:id
^^^^^^^^^^^^^^^^^^^^^^

ID represents unique ID of that entity in their supply chain.

urn:ot:object:actor:name
^^^^^^^^^^^^^^^^^^^^^^^^

Name is short string description of entity. It can represent full or
partial company name or full name of person.

urn:ot:object:actor:description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Description is long textual description of the entity that provides
broader context.

urn:ot:object:actor:category
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Category represents the type of the entity. Please try to choose value
from list below:

-  Person
-  Company
-  Other

urn:ot:object:actor:wallet
^^^^^^^^^^^^^^^^^^^^^^^^^^

Wallet represents Ethereum wallet of the entity that is used for their
DC node.

Product urn:ot:object:product
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Products (supply chain objects of interest), which represent goods or
services that are acted upon by actors (the “What”). Product is metadata
entity for Batch (batch is physical instance of product).

urn:ot:object:product:id
^^^^^^^^^^^^^^^^^^^^^^^^

ID represents unique ID of that entity in their supply chain.

urn:ot:object:product:description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Description is long textual description of the entity that provides
broader context.

urn:ot:object:product:category
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Category represents the type of the entity. List of product
classification can be found on `GS1 GPC standards`_. If the product is
wine, then value for category can be Beverage that is in GPC list as
part of the wider Food/Beverage/Tobacco category.

Batch urn:ot:object:product:batch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Batch represents physical instance of a product. Batch represents
minimal subset of products that have same characteristics. There are two
main types of batches that GS1 handles - lots and serial numbers. More
information about GTIN can be found `here`_.

urn:ot:object:product:batch:id
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ID represents unique ID of that entity in their supply chain.

urn:ot:object:product:batch:productId
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

productId represents explicit relation with batch and product. Value of
this attribute should contain value from attribute
urn:ot:object:product:id in corresponding entity.

urn:ot:object:product:batch:productionDate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Date when batch was produced.

urn:ot:object:product:batch:expirationDate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Date after which a product (such as food or medicine) should not be sold
because of an expected decline in quality or effectiveness.

Location urn:ot:object:location
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Locations, which define either physical or digital locations of products
or actors (the “Where”).

urn:ot:object:location:id
^^^^^^^^^^^^^^^^^^^^^^^^^

ID represents unique ID of that entity in their supply chain.

urn:ot:object:location:category
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Category represents the type of the entity. Please try to choose value
from list below:

-  Building
-  Readpoint
-  Vehicle
-  Other

urn:ot:object:location:description
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Description is long textual description of the entity that provides
broader context.

urn:ot:object:location:actorId
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

productId represents explicit relation with location and actor. Value of
this attribute should contain value from attribute
urn:ot:object:actor:id in corresponding entity. Value proclaims which
actor is the owner of that location.

Event urn:ot:event
------------------

Transport urn:ot:event:transport
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Transport events, which explain the physical or digital relocation of
objects in the supply chain.

Transformation urn:ot:event:transformation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Transformation events, which contain information about the
transformation of one or more objects into (a new) one. An example would
be the case of an electronic device (i.e. mobile phone), where the
assembly is observed as a transformation event of combining different
components – Objects - into one output Object, or the case of combining
a set of SKUs in one group entity such as a transportation pallet.
Similarly, a digital transformation event would be any type of
processing of a digital product (i.e. mastering of a digital sound
recording). This event type corresponds to GS1 AggregationEvents and
TransformationEvents.

Observation urn:ot:event:observation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Observation events, which entail any type of observational activity such
as temperature tracking via sensors or laboratory tests. This event
corresponds to GS1 ObjectEvents that are published by one party
(interaction between different business entities is not the primary
focus of the event).

Ownership urn:ot:event:ownership
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ownership/custody transfer events, where the change of ownership or
custody of Objects is distinctly explained. An example would be a sale
event. **Consensus check is only triggered on Ownership events by
documentID key value between Source and Destination owners.**

Extension
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

GS1 EPCIS standard allows custom extensions in Event section.
OriginTrail has following tags:

-  OTEventClass - values can be one of urn:ot:event members of
   namespace. 1:N tags are allowed.
-  OTEventType - value is string that describes process. 1:N tags are
   allowed.
-  documentID - value represents key for consensus check between
   participants. One event can have several documents in Business
   Transaction List, but only the documentId value will be used for link
   between two events that are described by different entities



.. _GS1 GPC standards: https://www.gs1.org/standards/gpc/dec-2017
.. _here: https://www.gs1.org/standards/id-keys/gtin
.. _GS1 identifiers: http://www.gs1mu.org/about-us/gs1-standards/gs1-system
.. _CBV document: https://www.gs1.org/sites/default/files/docs/epc/CBV-Standard-1-2-2-r-2017-10-12.pdf
.. _EPCIS standard: https://www.gs1.org/standards/epcis
.. _guidelines: https://github.com/OriginTrail/ot-node/wiki/Data-Structure-Guidelines
.. _data model: https://github.com/OriginTrail/ot-node/wiki/Graph-structure-in-OriginTrail-Data-Layer---version-1.0