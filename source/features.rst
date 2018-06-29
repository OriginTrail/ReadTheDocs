..  _features:

Features
======================================


Consensus check
---------------
When receiving information from stakeholders, OriginTrail protocol performs a **consensus check** that verifies there are no discrepancies between data provided by different
stakeholders. 

.. _ImageLink::https://github.com/OriginTrail/ReadTheDocs/raw/master/source/slide-interperability_and_data_integrity.png

The check is performed in several steps:

**Step 1.** Each stakeholder has to be approved by the previous and the following supply chain
stakeholder, creating a chain of accountability.

**Step 2.** Matching of dynamic batch information is verified, including the critical information
of batch identifiers, appropriate timestamps and transactional data. As this step involves
company private data (e.g. quantities of sales), a Zero Knowledge Proof mechanism
implementation will provide a way to check that private information matching is
provable without revealing the information itself. Other dynamic data may include data
collected from sensors and compliance data.

**Step 3.** As an additional layer of credibility, auditing and compliance organisations can
validate data by supplying their confirmations.

This ensures the entire supply chain is in accords regarding that batch of products. If there is
no consensus, discrepancies can be quickly reported, investigated and reconciled.
Reconciliation of discrepancies is also recorded on OriginTrail - the additional information is
uploaded as a special “reconciliation” data set which is again subject to the same consensus
mechanism.

To see the consensus check in action, after importing the data in the system, send a request to the API endpoint to get the trail of a certain batch of products and the response will provide you with the consensus parameters for each of the events in the product history trail.

Consensus check with Zero Knowledge proof
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This particular ZK implementation is meant to provide for the ability to validate the mass balance (quantities) within the observed supply chain by being able to validate that the inputs match the outputs in any arbitrary supply chain location of relevance (provided that, of course, data of the input and output is available), without revealing the values of those inputs and outputs.

The consensus check utilizes this functionality, while additionally performing other checks as well, essentially making sure that claims along with a certain observed supply chain match between all the parties involved, including that all the parties confirm cooperation in the first place. A consensus, in this case, happens when all the stakeholder pairs in the supply chain have matching claims as well as having cumulative values validated by the ZK algorithm.

Token staking and reputation
----------------------------
The incentive model introduces a novel form of a token staking mechanism implemented in order to discourage potential malicious behaviour within the network. It differs from the more common model of staking in decentralized networks where a certain, usually relatively large amount of tokens, is set as collateral in order to qualify as a validator (i.e. Ethereum Casper) or to perform a different set of services (i.e. Dash Tier 2 Masternode) within a system. In case of foul play, the staked amount can be lost, thus discouraging the actor to perform against the rules of the system.

A similar concept is introduced in OriginTrail on a level of individual agreement between nodes. In order to participate in data exchange, the nodes involved need to stake a proportional amount of tokens to the agreed upon amount in the exchange. Because any node (DC — data creator or DH — data holder) is involved in an arbitrary number of agreements, the amount of stake needed is a function of the agreements themselves. This staked amount is used to ensure the agreement success, as it further demotivates the involved parties to act against the system — the DH to remove or change the data it has committed to keeping and delivering, and on the other hand demotivate the DC node to falsely probe the DH nodes in order to try to obtain their stake.

This effectively means that in order to take on more work in the system and thus obtain more token gains and a larger reputation, a node needs to provide a larger cumulative stake, which ensures aligned incentives and motivation to deliver high quality of service.

The elasticity of this system allows both large and smaller token holders to participate on their own terms by being able to “monetize” their reputation and stake if they have a good historical track record in OriginTrail, but without limiting smaller players from entering the system and providing high-quality service. In this way, a data provider can choose to require a certain high level of reputation and stake (which would guarantee a higher degree of trust) from the nodes, which will in return be valued at a higher service price. In this way, the system stays decentralized and the distribution of data remains wide.

Bidding system
--------------
one of the main building blocks of ODN is the bidding mechanism by which nodes in the network are able to form agreements and bid on providing services to each other. The compensation happens on the relation between the data creators (DC) nodes (responsible for replication) and data holder (DH) nodes, utilizing our Test-Answer-Receipt (TAR) protocol. This protocol essentially mimics the typical HTTP handshake and presents a testing mechanism between the nodes that are providing OriginTrail network services. Each test presents a challenge for the DH node, which, when solved correctly, provides an answer to the DC node. The DC node then, in return, provides a valid receipt for the service, according to the predefined “deal” (service conditions, price, and data lifespan). The compensation is then handled according to the results of the test and allows the DH node to independently collect tokens from a Service escrow smart contract by providing valid receipts to it.

This implementation of the bidding system is based on an Ethereum smart contract. In the bidding system, an Ethereum smart contract handles the creation of Offers by Data Creator (DC) nodes. DC nodes receive Bids from Data Holder (DH) nodes should an Offer become interesting to one or more DH node.

Bids are submitted in a sealed form, so, the content of the bids — the number of tokens and stake offered — are not visible to any of the participants during the bidding process. Once bids are revealed, new bids cannot be submitted. The DC node then initiates a choice mechanism, which runs a “roulette” function with associated probabilities of choice corresponding to the number of tokens and stake provided in the bid by each DH node. Once a choice is made, the tokens needed for the agreements are transferred to the Escrow smart contract facilitating the payment mechanism.
This initial implementation comes with certain simplifications and limitations, which will be improved in the following releases before we launch the testnet. Currently, the bidding mechanism disregards the node reputation scores. For now, the bidding system takes the amount of stake and tokens involved in the bid for a simple “roulette” choice, which allocates higher probabilities to the DH nodes that have applied with a higher stake and token price. The roulette then randomly selects the applied nodes based on these probabilities and checks if they have the tokens to pay for the contract. Furthermore, the replication size is currently limited by the smart contract gas running limits, so, it will not work for a large replication number (hundreds of nodes or more).

In future development, we plan to move several operations off-chain to the ODN network layer, leaving only the most important (trusted) operations to be performed by the contract itself. While Kosmos introduces the initial version of the bidding mechanism, the final mechanism is subject to a longer process of iterative research and development, as well as a thorough and lengthy testing in the upcoming period before we launch the testnet in June. The bidding mechanism will be gradually improved, based on real transaction data from multiple companies. There is other scheduled work to be done by the launch of testnet, with several iterations and more fine-tuning of code, logic and game theory behind the mechanism.

Simply put, the Data Creator node (DC), the one introducing new data to the network, forms agreements with Data Holder nodes (DH) to operate on and store data (D) on a particular observed supply chain (S). For the specific data set D, a set of agreements is made between the DC of the data provider, and several DH nodes, among which are both independent nodes within the network, as well as the associated partner nodes of the data provider entity. In that regard, it is important to understand how a node agreement is formed.
 
.. image::https://github.com/OriginTrail/ReadTheDocs/raw/master/source/slide-system_overview%402x.png
 
To form the set of agreements (A) associated with one data set D, the DC node of the data provider creates an initial offer (O). This offer contains the parameters set by the DC node such as:

the maximum amount of tokens the DC node is willing to provide as reimbursement per data unit for DH nodes,
the minimum amount of required stake for the agreement to happen,
the amount of time the agreement will last and
a minimum reputation requirement for the DH nodes.
In previous releases containing the initial version of the bidding mechanism, the actual bidding was performed in a type of a blind auction during which each of the interested DH nodes applying for the offer O would send an encrypted amount. This amount would be revealed in the next step to mitigate the risk of nodes undercutting each other in the race. The final list of applicants would then be associated with a set of probabilities according to the parameters the nodes have applied with to the offer, which would then be utilized in a roulette type of random choice function. This system had its foreseen downsides as it didn’t scale for a large number of DH applicants, and because it had a cumbersome revealing period which was increasing complexity and cost of the mechanism.

The improved version in Surveyor utilizes a different approach which allows for DH nodes to apply with a pre-revealed bid if the node itself estimates that there is a high probability of being included in the agreement set. The important enabling change is that this probability is determined by the distance function used to rank all DH candidates, which incorporates all the necessary parameters of the offer, as well as the address space distance of the node address from the address of the data content hash. In this way, there is a mechanism with less complexity (no revealing needed and no complicated and bounded roulette) and with a fair density of data dissemination determined solely by the data itself. There will be several improvements and tweaks to the new mechanism as soon as there has been enough time to collect observations and derive conclusions on better parametrization.

The payment mechanism is now extended to support the ability to perform trustless, monetized data reading from the OriginTrail Decentralized Network (ODN). In this way, the data creator (DC) and data holder (DH) nodes will be able to charge a fee from data viewer (DV) nodes, which would read data from them in order to provide them with the requested data. The payment mechanism enables many different operations to be built and we are looking forward to seeing it being used in the testnet phase, as it is still a novel concept and will surely provide interesting insights, valuable to future business case development.

Privacy layer
-------------

As we have entered the final phases of the alpha development period, we are able to take the observations over the previous period and incorporate the findings into the development roadmap as we go. We have, so far, iterated successfully on several components of the system — the bidding mechanism, privacy layer, underlying database systems, network communication and importer. Explorer now supports more features on the privacy layer, which includes the zero-knowledge algorithm published a month ago in Zond. It brings the ability to handle private data within the system in such a way that the owner can retain control of the information by their DC (data creator) node, while publishing cryptographic commitments in the system to the DH (data holder) nodes involved in replication. This first iteration is just the beginning of further developments in the privacy layer, which is one of the most important components of the OriginTrail protocol.

Zero knowledge proof
--------------------

One of the major problems we have identified in more than seven years of working in the industry is the ability to validate that a supply chain has a consistent balance when it comes to the quantity or mass of the raw materials and semi-products moving through the chain.

There are several reasons for this:

The rising complexity of supply chains, which are, realistically speaking, supply chain networks;
The data fragmentation within “data silos” of participating stakeholders, and, finally;
The reluctance to share sensitive information which might be used in a negative context in the market against the one sharing such information.
A typical example of such information would be the quantities of sold goods in certain markets, which could be used by competitors in ways counterproductive to the party sharing this information in the first place.

That is why establishing an open-source collaborative protocol such as OriginTrail must not only tackle the problems of data integrity and interoperability by providing a platform neutral, non-proprietary decentralized network tailored for supply chain data sharing, but also provide a way to unlock value from data that is essentially not meant to be shared. So, how does this work? Let’s provide a simplified example.

Let’s assume we have a dairy company buying raw milk from two dairy farms. The first dairy farm provides an A quantity of milk while the second provides a B quantity. The result of the production process, if there is no foul play, would, in simplified terms, be a batch of milk with quantity C, derived through the addition of the A and B quantities. Because we are talking about a food supply chain, this batch of milk with quantity C would continue moving along the chain and parts of it would likely end up at many retail stores. Ideally, if we added up all these different parts that ended up at different retail stores, they would equal the same amount of C = A + B. Again, this is a simplification, as processing, spillage and other factors have to be considered, though this does not hinder the ability of the system to cope with such situations.

Representation of a singled out supply chain event of producing a quantity of C milk out of raw materials A and B
Today, it is not easy to account for all parts of a particular raw material quantity in supply chains, and there are many cases of foul play, especially when it comes to organic food. It is really hard to make sure irregular, non-organic products, are not getting added to organic ones and being sold off as organic, higher value products. Again, this is the result of informational asymmetry as the stakeholders in the market are not able to validate the whole chain, of which one major part is the ability to validate mass balance and quantities.

..image:https://raw.githubusercontent.com/OriginTrail/ReadTheDocs/master/source/zk1.JPG

How do we then enable this data sharing to happen when there’s no incentive to share this information? The privacy layer in ODN is designed to provide a “zero-knowledge” way for validating these data elements in successive events in the supply chain. Zero knowledge protocols in general terms provide a way for an interested party — the “verifier” — to successfully verify that the observed party — the “prover” — has knowledge about a specific piece of information — “truth” — without revealing the “truth” itself. When it comes to the OriginTrail zero-knowledge implementation, this means that the companies would be able to share quantities A,B and C in specially encrypted forms E(A), E(B) and E(C), and any observer, aka “verifier,” would be able to confirm whether these values correctly fit the validation equation E(A) * E(B) = E(C). The verifier cannot obtain the values of A, B and C, but is able to confirm that the quantity input and output of a certain event or process in a supply chain is valid. Consequently, if there was some mismatch and E(A) * E(B) would not equal E(C), that would mean that there exists some integer quantity D for which A + B = C + D and thus E(A) * E(B) = E(C) * E(D).

.. image::https://raw.githubusercontent.com/OriginTrail/ReadTheDocs/master/source/zk2.JPG

Validation is performed on encrypted values, keeping original quantities hidden
This would provide for a valuable insight to everyone involved in the supply chain as it would provide a starting point for investigation into what has happened. In several cases so far we have observed quantity mismatches due to plain data inconsistencies regarding bookkeeping with companies we have worked with. These inconsistencies were revealed by the OriginTrail protocol and have helped them fix their internal data handling. Having said that, the quantity D can be manifested as an error in accounting, as well as a potential supply chain misbehavior. By repeating the process along the whole supply chain network, the system allows for full validation of quantity matching in the chain, without exposing sensitive information and thus unlocking major value from the previously siloed and unshareable data.

It is important to state that this implementation of the zero knowledge protocol is specially tailor-made for the use case of supply chains, so it is quite different from other zero-knowledge implementations seen in other systems like Z-Cash.

The mathematical basis of the implementation can be found here. The first iteration of the implementation allows for establishing checks on transformational events in the supply chain. Currently the validation is performed at import runtime and can be observed in the logs for each event. The proofs are generated for every event and validated by the importer but equality of proofs of ownership transfer events between providers can be validated manually.

When it comes to the zero-knowledge implementation in the ODN, the Lunar Orbiter now supports quantity validation across several events in the observed supply chain, with the ability to have them be reported in arbitrary stages of their execution, across multiple XML files. This is an important improvement from the previous version and presents the first full implementation of the zero-knowledge quantity balance mechanism. To utilize the feature, the GS1 XML creation needs to be updated to support it, and will also be explained in detail in our documentation.

Data fingerprinting
-------------------

The fingerprinting functionality has also been upgraded to utilize Merkle tree hashing in order to allow for flexible blockchain layer validation. It is now possible to fingerprint a graph of arbitrary size on the Ethereum blockchain, which allows for fine-tuning the tradeoff between storing less fingerprints per kilobyte (to save on ETH) and requiring lighter reads from the system in order to validate the integrity of the information.

At this moment, all the blockchain functionality is being tailored for Ethereum, but the code is structured in a way that abstracts (virtualizes) the blockchain implementation. This means that interfaces can be written to other blockchains without requiring changes to the rest of the system. This could provide a lot of value to the protocol. Becoming less dependent on a single chain could make the protocol attractive for markets that prefer non-Ethereum blockchains, and bring robustness and potential for lowering cost should one of the mainstream blockchains become highly volatile for some reason.
