..  _import-data:

Import data
============

In order to make the initial import there are prerequisits:

-  data has to be properly formatted. Further description of data standards can be found in :ref:`data-structure-guidelines`. 
- Node installation includes `example`_ files in the project for your reference and testing. If you want to test import on them, just replace wallet in the file with wallet of your node. 

Once data is ready, import process can be executed on several ways ODN:

- Please check :ref:`introduction-to-api` page  for detailed instuctions.

Depending on the use case, we strongly recommend to set up a periodic
import of the data into ODN (i.e. a daily cronjob exporting the
data from your ERP in the data format requested).

.. _example: https://github.com/OriginTrail/ot-node/tree/develop/importers/xml_examples