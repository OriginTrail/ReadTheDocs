Configuration parameters for node
==================================

**identity**

* Default value: null
* Description:
* Note:

**node_wallet**

* Default value: null
* Description: The address of the operational wallet that the node will use for dataset signing, creating and finalizing offers, payouts, and litigation initiating, answering, and completing. Hex string with the 0x at the beginning.
* Note:

**node_private_key**

* Default value: null
* Description: The private key corresponding to the node_wallet. Hex string without 0x at the start.
* Note:

**management_wallet**

* Default value: null
* Description: The value of the management wallet to be used if a new ERC725 identity will be created.,"This value is used when the new identity is generated and from that point on it is not used, so changing it after the ERC725 identity is generated will not change it on the blockchain.
* Note:


**houston_password**

* Default value: null
* Description: The value of the authentication password used for authenticating requests incoming from the Houston app,This value is read from the houston_password_filename file or generated if the file does not exist. Setting it directly will do nothing since it will be overriden upon node startup
* Note:


**houston_password_file_name**

* Default value: houston.txt
* Description: The file in which the houston_password is stored
* Note:


**node_port**

* Default value: 5278
* Description: The port used for netwok layer communication with other nodes
* Note:

**node_rpc_ip**

* Default value: 127.0.0.1
* Description: The ip address on which the node's RPC server will be set up
* Note:


**node_rpc_port**

* Default value: 8900
* Description: The port value on which the node's RPC server will be listening
* Note:

**remote_control_enabled**

* Default value: TRUE
* Description: A flag determining whether the connection with the Houston app will be enabled
* Note:


**node_remote_control_port**

* Default value: 3000
* Description:
* Note:


**is_bootstrap_node**

* Default value: FALSE
* Description: A flag determining whether the node will run as a bootstrap node or not
* Note:

**send_logs**

* Default value: FALSE
* Description: A flag determining whether to send the logs to OriginTrail log monitoring service
* Note:

**logs_level_debug**

* Default value: TRUE
* Description: A flag determining whether to display debug level logs
* Note:

**traverse_nat_enabled**

* Default value: FALSE
* Description: A flag determining whether NAT traversal should be enabled on the node
* Note:


**reverse_tunnel_address**

* Default value: diglet.origintrail.io
* Description: The tunnel address for the NAT traversal on the node
* Note:


**reverse_tunnel_port**

* Default value: 8443
* Description: The tunnel port for the NAT traversal on the node
* Note:


**request_timeout**

* Default value: 20000
* Description: The timeout for the node's network requests
* Note:

**ssl_keypath**

* Default value: kademlia.key
* Description: The path to the file containing the node's network layer SSL key
* Note:


**ssl_certificate_path**

* Default value: kademlia.crt
* Description: The path to the file containing the node's network layer SSL certificate
* Note:

**identity_filepath**

* Default value: identity.json
* Description: The path to the file containing the node's network layer identity
* Note:


**erc725_identity_filepath**

* Default value: erc725_identity.json
* Description: The path to the node's erc725 identity
* Note: If the file is missing, the node will create a new file and try to create a new identity on the blockchain


**cpus**

* Default value: 0
* Description: Deprecated
* Note:


**embedded_peercache_path**

* Default value: peercache
* Description: The path to the peercache file used for network layer communication
* Note:


**onion_virtual_port**

* Default value: 4043
* Description: Deprecated
* Note:


**traverse_port_forward_ttl**

* Default value: 0
* Description: Deprecated
* Note:


**verbose_logging**

* Default value: FALSE
* Description: Deprecated
* Note:


**control_port_enabled**

* Default value: FALSE
* Description:
* Note:

**control_port**

* Default value: 5279
* Description:
* Note:

**control_sock_enabled**

* Default value: FALSE
* Description:
* Note:

**control_sock**

* Default value: 12000
* Description: Deprecated
* Note:

**ssl_authority_paths**

* Default value: []
* Description:
* Note:

**send_logs_to_origintrail**

* Default value: FALSE
* Description:
* Note:

**read_stake_factor**

* Default value: 1
* Description: Deprecated, but used in network query and read
* Note:

**dh_min_stake_amount**

* Default value: 100000000000
* Description: Deprecated
* Note:


**dh_min_reputation**

* Default value: -50
* Description: The reputation value below which a node will not accept replication requests from a holder
* Note:


**latest_api_version**

* Default value: v2.0
* Description: Which api version to use when api requests are sent with version set to latest
* Note:


**default_data_price**

* Default value: 100000000000000000000
* Description: The default price for a permissioned data object (not used on mainnet yet)
* Note:


**send_challenges_log**

* Default value: TRUE
* Description: A flag determining whether challenge answers will be sent to OriginTrail monitoring service
* Note:


**send_logs**

* Default value: FALSE
* Description: A flag determining whether to send the logs to OriginTrail log monitoring service
* Note:

**logs_level_debug**

* Default value: TRUE
* Description: A flag determining whether to display debug level logs
* Note:

**traverse_nat_enabled**

* Default value: FALSE
* Description: A flag determining whether NAT traversal should be enabled on the node
* Note:


**reverse_tunnel_address**

* Default value: diglet.origintrail.io
* Description: The tunnel address for the NAT traversal on the node
* Note:


**reverse_tunnel_port**

* Default value: 8443
* Description: The tunnel port for the NAT traversal on the node
* Note:


**request_timeout**

* Default value: 20000
* Description: The timeout for the node's network requests
* Note:

**ssl_keypath**

* Default value: kademlia.key
* Description: The path to the file containing the node's network layer SSL key
* Note:


**ssl_certificate_path**

* Default value: kademlia.crt
* Description: The path to the file containing the node's network layer SSL certificate
* Note:

**identity_filepath**

* Default value: identity.json
* Description: The path to the file containing the node's network layer identity
* Note:


**erc725_identity_filepath**

* Default value: erc725_identity.json
* Description: The path to the node's erc725 identity
* Note: If the file is missing, the node will create a new file and try to create a new identity on the blockchain


**cpus**

* Default value: 0
* Description: Deprecated
* Note:


**embedded_peercache_path**

* Default value: peercache
* Description: The path to the peercache file used for network layer communication
* Note:


**onion_virtual_port**

* Default value: 4043
* Description: Deprecated
* Note:


**traverse_port_forward_ttl**

* Default value: 0
* Description: Deprecated
* Note:


**verbose_logging**

* Default value: FALSE
* Description: Deprecated
* Note:


**control_port_enabled**

* Default value: FALSE
* Description:
* Note:

**control_port**

* Default value: 5279
* Description:
* Note:

**control_sock_enabled**

* Default value: FALSE
* Description:
* Note:

**control_sock**

* Default value: 12000
* Description: Deprecated
* Note:

**ssl_authority_paths**

* Default value: []
* Description:
* Note:

**send_logs_to_origintrail**

* Default value: FALSE
* Description:
* Note:

**read_stake_factor**

* Default value: 1
* Description: Deprecated, but used in network query and read
* Note:

**dh_min_stake_amount**

* Default value: 100000000000
* Description: Deprecated
* Note:


**dh_min_reputation**

* Default value: -50
* Description: The reputation value below which a node will not accept replication requests from a holder
* Note:


**latest_api_version**

* Default value: v2.0
* Description: Which api version to use when api requests are sent with version set to latest
* Note:


**default_data_price**

* Default value: 100000000000000000000
* Description: The default price for a permissioned data object (not used on mainnet yet)
* Note:


**send_challenges_log**

* Default value: TRUE
* Description: A flag determining whether challenge answers will be sent to OriginTrail monitoring service
* Note:

***************
Database
***************

**provider**

* Default value: arangodb
* Description: The provider used for the ot-nodes graph database
* Note: Currently only supports arangodb


**username**

* Default value: root
* Description: The username for accessing the graph database
* Note:


**password**

* Default value: root
* Description: The password for accessing the graph database
* Note:

**password_file_name**

* Default value: arango.txt
* Description: The file path where the non default password will be saved after the node is first set up
* Note:

**port**

* Default value: 8529
* Description: The port for accessing the graph database
* Note:

**database**

* Default value: origintrail
* Description: The name of the database used for ot-node
* Note:

**host**

* Default value: localhost
* Description: The ip address for accessing the graph database
* Note:

**max_path_length**

* Default value: 100
* Description: The default maximum path length for local graph traversals
* Note:

**********
Blockchain
**********

**blockchain_title**

* Default value: Ethereum
* Description: The blockchain network used by the node
* Note: Currently only supports Ethereum


**network_id**

* Default value: mainnet
* Description: Determines which blockchain network is used by the node
* Note: This parameter is used by the node when verifying dataset fingerprints from the blockchain


**gas_limit**

* Default value: 2000000
* Description: The amount of gas sent for each transaction, i.e. the maximum amount of gas a transaction can use
* Note: The 2000000 number is necessary only for creating a new profile and identity, after that it's safe to decrease this number to 1000000


**gas_price**

* Default value: 20000000000
* Description: Gas price used if network gas price is unavailable
* Note:


**max_allowed_gas_price**

* Default value: 50000000000
* Description: The maximum allowed gas price for a transaction.
* Note: If the network average is above this number, the node will delay sending the transaction.


**dc_price_factor**

* Default value: 6
* Description: The lambda factor used when the data creator is calculating the offer price
* Note:


**dh_price_factor**

* Default value: 5
* Description: The lambda factor used when the data holder is calculating their minimum acceptable offer price
* Note:

**trac_price_in_eth**

* Default value: 0
* Description: A factor used for calculating the offer price.
* Note: This parameter is dynamically loaded from an external service, so it gets updated upon node startup, changing it won't make a difference

**hub_contract_address**

* Default value: 0x89777F4D16F0a263F47EaD07cbCAb9497861aa79
* Description: The address of the Hub smart contract on the current blockchain network
* Note:

**enabled**

* Default value: FALSE
* Description: A flag determining whether or not a blockchain plugin is to be used
* Note:

**provider**

* Default value: Hyperledger
* Description: The provider of the blockchain plugin
* Note:

**name**

* Default value: fingerprint-plugin
* Description: The name of the blockchain plugin
* Note:

**url**

* Default value: URL
* Description: The URL used for the blockchain plugin
* Note:


**user**

* Default value: user
* Description: The username used for the blockchain plugin
* Note:


**pass**

* Default value: pass
* Description: The password used for the blockchain plugin
* Note:

*******
Network
*******

**hostname**

* Default value: 127.0.0.1
* Description: The hostname of the node to be used on the network layer.
* Note:



**id**

* Default value: MainnetV4.0
* Description: The identifier of the network the node is a member of.
* Note: The node rejects messages that arrive from networks with a different id


**bootstraps**

* Default value: "[
        ""https://mainnet-bootstrap-wm-monica-borer-21.origin-trail.network:5278/#61a8b70d373c8c64a1cecf2602b9176399ace9ca"",
        ""https://mainnet-bootstrap-wm-wilbert-bode-41.origin-trail.network:5278/#a9a53281f99538c3e23cf0b2726cfd5901611ff5"",
        ""https://mainnet-bootstrap-wm-dortha-metz-70.origin-trail.network:5278/#5d462772267b8ae0e4fdf52397a370439f338c9d"",
        ""https://mainnet-bootstrap-wm-domenick-goldner-5.origin-trail.network:5278/#ae3896d7cf0a52f3dddeef80b5243695dce88cdf"",
        ""https://mainnet-bootstrap-wm-kaleigh-schaefer-47.origin-trail.network:5278/#95f54eb6bc713706e2ec76881ea11db390cad51d""
      ]"
* Description: Nodes to which the node will connect upon first setup
* Note:


**remoteWhitelist**

* Default value: ["127.0.0.1"]
* Description: The list of IP addresses which are allowed to send API requests to nodes
* Note:


**solutionDifficulty**

* Default value: 14
* Description: The network identtity difficulty parameter, determining the difficulty of proof-of-work required for generating a network identity
* Note:


**identityDifficulty**

* Default value: 12
* Description: Deprecated
* Note:

**bucket_size**

* Default value: 20
* Description: The size of a network contact bucket
* Note:


**node_rpc_use_ssl**

* Default value: ""
* Description: The path to the node's SSL key file
* Note:


**node_rpc_ssl_key_path**

* Default value: ""
* Description: The path to the node's SSL certificate file
* Note:


**node_rpc_ssl_cert_path**

* Default value: FALSE
* Description: A flag determining whether the node will run as a bootstrap node or not
* Note:

**bugsnag_releaseStage**

* Default value: "mainnet"
* Description: A flag determining which environment the bug originated from when errors are sent to the OriginTrail monitoring service
* Note:

************
Churn Plugin
************

**cooldownBaseTimeout**

* Default value: 5m
* Description: The period of time node will avoid a contact if the correspondent failed to answer
* Note:


**cooldownMultiplier**

* Default value: 2
* Description: The multiplier for the timeout period if the node fails to answer repeatedly
* Note:

** cooldownResetTime**

* Default value: 1s
* Description: The reset time for avoiding a contact
* Note:


************
Auto Updater
************


**enabled**

* Default value: TRUE
* Description: A flag determining whether the node will automatically update or not
* Note:

**packageJsonUrl**

* Default value: "https://raw.githubusercontent.com/OriginTrail/ot-node/release/mainnet/package.json"
* Description: The package.json file on the ot-node repo which the node will check for updates
* Note:

**archiveUrl**

* Default value: "https://github.com/OriginTrail/ot-node/archive/release/mainnet.zip"
* Description: The repository zip file which the node will download when updating
* Note:

**dataSetStorage**

* Default value: FALSE
* Description: A flag determining whether the node will run as a bootstrap node or not
* Note:

***************
Dataset Storage
***************

**dc_holding_time_in_minutes**

* Default value: 262800
* Description: The value used for holding time for a new offer if the holding time is not submitted in the api
* Note:


**dc_litigation_interval_in_minutes**

* Default value: 15
* Description: The value used for the litigation interval when the node creates a new offer
* Note:


**dc_challenge_retry_delay_in_millis**

* Default value: 600000
* Description: The delay before a node retries to send a challenge to a holder if it failed to contact the holder
* Note:


**dh_max_holding_time_in_minutes**

* Default value: 5256000
* Description: The maximum time a node will accept for a holding job.
* Note:


**dh_maximum_dataset_filesize_in_mb**

* Default value: 3
* Description: The maximum size a node will accept for a holding job.
* Note:


**dh_min_litigation_interval_in_minutes**

* Default value: 5
* Description: The minimum litigation interval a node will accept for a holding job
* Note:


**dc_choose_time**

* Default value: 600000
* Description: The time a data creator will wait for replication requests before it tries to finalize an offer.
* Note:

**requireApproval**

* Default value: FALSE
* Description: Whether or not the node will only communicate with nodes which are approved.
* Note:


**litigationEnabled**

* Default value: TRUE
* Description: A flag determining whether a node will send challenges (and possibly litigate) a holder for a new job
* Note:


**commandExecutorVerboseLoggingEnabled**

* Default value: FALSE
* Description: A flag determining whether to display the debug-level logs of the command executor
* Note:


**reputationWindowInMinutes**

* Default value: 12960
* Description: The timeframe for which the node will keep reputation data about other nodes.
* Note: