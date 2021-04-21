Configuration parameters
========================

The ot-node has many configuration parameters which change how the node behaves and what it's identity on the network is.

.. note::

    This page currently does not cover all parameters. The intention is to over time cover all
    available configuration parameters, but at the time of writing it's a work in progress.

.. important::

    Some numeric values are specified as strings, while others are specified as numbers. Please be careful to use the
    correct format for those parameters, otherwise it might cause issues with your node.

Dataset pruning section
-----------------------

This section enables the node to remove datasets which have expired, freeing up space for new datasets on the network.
The ``dataset_pruning`` parameter should be specified in the root level of the configuration object.

The structure and the default values for the section are shown below:

.. code:: json

    {
        "dataset_pruning": {
            "enabled": false,
            "imported_pruning_delay_in_minutes": 1440,
            "replicated_pruning_delay_in_minutes": 1440
        }
    }


The ``enabled`` parameter is a boolean value which determines whether the datasets should be pruned or not. If the
pruning feature is enabled the node will check every 24 hours which datasets should be pruned and remove them from the
node's graph database and remove corresponding data from its operational database.

The ``replicated_pruning_delay_in_minutes`` parameter is a number value and determines how much the node should wait
after all offers for a dataset have completed (the holding time for the offers has passed) before pruning the dataset.
This is used for datasets which data holder nodes have received over the network and which data creator nodes have
replicated.

The ``imported_pruning_delay_in_minutes`` parameter is a number value and determines how much the node should wait after
importing a dataset before pruning it.
This is used for datasets which data creator nodes have imported but have not replicated on the network.

