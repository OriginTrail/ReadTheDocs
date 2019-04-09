..  _node-setup-ssl:

Setting up SSL on a node
========================

In order to setup the SSL connection for node's remote API make sure you have prepared a certificates and
registered a domain. Once you enable secure connection it will be used for both API (default port 8900) and
remote control (default port 3000). If you are using different ports than the defaults make sure you map
them correctly during container initialization.

Prerequirements
~~~~~~~~~~~~~~~

Make sure your certificates are in PEM format stored locally so you can provide them to the node or a
Docker container running node.

Configuration
~~~~~~~~~~~~~

Let's assume that the certificates of your domain (for example: my.domain.com) are stored in /home/user/certs.
In that dir there should be the *fullchain.pem* and *privkey.pem* file.

Edit the node's configuration file and make sure it has following items in the JSON root:

.. code:: json

        "node_rpc_use_ssl": true,
        "node_rpc_ssl_cert_path": "/ot-node/certs/fullchain.pem",
        "node_rpc_ssl_key_path": "/ot-node/certs/privkey.pem",

With this we're telling node to find certificate at following path: */ot-node/certs/* and that's where we're
going to leave them in the container.

Now create the docker container and mount cert dir into the container. We can achieve this by adding additional
parameters '-v /home/user/certs:/ot-node/certs/' to the container creation command. For example the
initialization of the Docker container for the OT node for the Mainnet could look like this:

.. code:: shell

        $ sudo docker run -i --name=otnode -p 8900:8900 -p 5278:5278 -p 3000:3000 -v /home/user/certs:/ot-node/certs/ -v ~/.origintrail_noderc:/ot-node/.origintrail_noderc origintrail/ot-node

After this the running container will be able to find certificate files at '/ot-node/certs/' location.
