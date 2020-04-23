Vertex Data permissioning
=========================

In cases when disclosing the full graph data publicly is not applicable to the implementation, it is possible to attach permissioned data to graph vertices, so that  trusted identities with permission will be able to read and verify it. This kind of functionality is possible through the OriginTrail protocol by using the permissioned data property.

Example of a permissioned data object can be found \ `here <https://github.com/OriginTrail/ot-node/blob/develop/importers/use_cases/marketplace/permissioned_data_simple_sample.json>`__\ .

Currently, permissioned data is only supported for datasets using the OT-JSON standard. By adding a \ ***permissioned\_data***\  attribute to any object in the ***@graph***\  array, that data will not be shared with an arbitrary data holder when replicated. Instead, a merkle root hash will be created of the data inside the permissioned data object in order to ensure data integrity. If the original data creator discloses the contents of the permissioned data, it’s integrity can be verified since the appropriate Merkle root of the data was published to the OriginTrail Decentralized Network.

If a data creator wishes to share permissioned data with a trusted third party, it can enable (whitelist) the specific Decentralized identity (a ERC725 node identity, but compatible with upcoming identity standards such as DID and SSI framework) to be able to view this data upon network read (see our API for more information). Although sharing of the permissioned data between parties is enabled by the protocol based on the decentralized identity authentication scheme, the protocol doesn’t govern the usage of the data after it has been accessed.

Permissioned data trading and monetization features are currently in development, with support for blockchain purchase verification by implementing the \ `FairSwap <https://eprint.iacr.org/2018/740.pdf>`__\  blockchain protocol.
