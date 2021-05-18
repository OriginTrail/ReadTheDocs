Configuration parameters
========================

The ot-node has many configuration parameters which change how the node behaves and what it's identity on the network is.

.. note::

    This page currently does not cover all parameters. The intention is to over time cover all
    available configuration parameters, but at the time of writing it's a work in progress.

.. important::

    Some numeric values are specified as strings, while others are specified as numbers. Please be careful to use the
    correct format for those parameters, otherwise it might cause issues with your node.

.. dataset_pruning

Dataset pruning section
-----------------------

How to check your node's storage usage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

    The dataset pruning feature is a new addition to the ot-node and we would like to hear from you if you're experiencing
    unexpected behaviour. This section will explain how to check your node's storage usage so you can identify unexpected
    behaviour and inform us so we can fix any edge cases.

To check your nodes storage usage run the following two commands

.. code:: bash

    du -h --max-depth 0 /var/lib/docker/
    docker system df

Take note of how much space the docker and docker containers are using. Now enable the dataset pruning feature in your node
configuration and restart your node. Wait until your node shows a log line stating ``Sucessfully pruned XYZ datasets``,
then measure your new storage usage with the two previous commands.

If your node has pruned more than a hundred datasets and your node storage usage hasn't changed, make sure your node is
running and execute ``docker system prune -a -f`` to prune unused docker data. Along with that you can restart the server
your node is running on. If the usage still remains the same, please contact us at `tech@origin-trail.com <mailto:tech@origin-trail.com>`__
with the subject **"Dataset pruning storage issue"**.

Dataset pruning section explained
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section enables the node to remove datasets which have expired, freeing up space for new datasets on the network.
The ``dataset_pruning`` parameter should be specified in the root level of the configuration object.

.. warning::

    Because of their opposite behaviour, ot-node restore process does not work well when the dataset pruning feature is
    enabled. Because of this, we strongly recommend disabling the dataset pruning feature before you run a node restore
    process, then re-enable the feature after the restore process has been completed.

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

