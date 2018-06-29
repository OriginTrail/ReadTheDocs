..  _implementation:

Implementation
======================================


Setting up a Project on OriginTrail
-----------------------------------
Here is the short overview how to set up a project on ODN. This document is aimed towards enterprise and solution provider entities.

Step 1 Structuring a use case
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Defining, structuring and formalizing use case is the most important step for the implementation as it drives all the other steps forward. In this step it is important to define the process flow, all the goals, all the stakeholders and all the data needed for OriginTrail. It is important to define which data will be utilized and visible on the end and in what way. 

-  Things needed for consideration when structuring a use case:
-  Define challenges of the problem and the solution proposition
-  Define the short and long term vision and what exactly is the purpose of the solution
-  Define who are the participants of the solution and their scope
-  Define key value propositions for every individual in the supply chain
-  Outline the process, describe formal process flow steps and identify data points that will trigger events for OriginTrail protocol
-  Define OriginTrail features that will be used (Consensus check, Zero Knowledge Protocol…).
-  Define the data sets that is required for processing
-  Define who is beneficiary of the end solution (who will use the data stored on ODN)
-  How will the data on ODN be utilized on frontend solution

The project should be structured into phases to ensure smooth transition of the project and to test the initial ideas set in the use case. There are three important phases in this process:

-  POC Exploration - Make proof of concept for proving the validity of the use case
-  Test Phase - Test the project with larger set of stakeholders
-  Live phase - Monetize the solution

Each of these phases need to have defined steps for easier tracking of the project, where the important aspects consist of:

-  Stakeholders - Who is involved in each phase
-  Responsibilities - what is the role of each stakeholder and what activities each needs to perform
-  Required data - What is the required data each stakeholder needs to provide  
-  Time-frame of the phase - How long each phase takes 

For the POC phase it is important that is structured accordingly and after all the aspects of the use case have been defined, the project can move forward to next steps.

Step 2: Getting raw data and structuring the sample files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once you identify which data you need along the process described in use case, primary goal is to align the data structure from all the stakeholders in the supply chain. Since different supply chain entities use different IT systems (SAP, Navision, Oracle,...) these data sets must be unified, in order to take advantage of the relational nature of the supply chain data.

These data sets must be structured according to OriginTrail Implementation Kit. If data source is ERP system of entity then GS1 EPCIS data standard should be used. The output of this step is an structured sample XML file. If data source are IoT devices, then Web of Things data standard should be used (EPCIS is also valid).

Steps in the process:

-  Identify data source and data owner (ERP system or IoT devices)
-  Acquire the data samples from each data source from each stakeholder along the supply chain in data points that are defined in use case
-  Define and resolve data privacy issues (what data is sensitive, what should be encrypted etc.)
-  Each company must structure data according to the instructions in the Implementation Kit, provided by OriginTrail.

Outcome of this step is a sample file which is should an generated with export integration procedure from the existing company IT system

Step 4: Installing and setting up a node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Node requirements and installation steps are specified in the implementation kit. This step also includes setting up the wallet for Ethereum and TRAC tokens. Installation sets up a node software and makes it part of ODN network. This step requires intermediate IT skills. 

Steps in the process:

-  Setting up a dedicated server
-  Setting up a Node software
-  Setting up a wallet

Step 5: Putting the data through Node
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In this step you need to put the sample data from step 3, to the installed node from step 4. Once you do that, the protocol takes care of the replication part and the Blockchain connection. In this way, your data will be stored in a cost efficient, decentralised way with a connection to Blockchain layer (Data is on ODN).

Important part of this step is that the company that wishes to put data on ODN, will need TRAC token to compensate the node holders (where the data will be stored). Price of the transaction will be determined by the node holders based on the volume of data and the time needed for data storage.

Step 6: Integration of OriginTrail with enterprise IT system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Integration is establishing communication from participants ERP to ODN. Communication is done through exchange of standardized and structured files. Exchange can be done through upload of the file by using web services or manually. Since data structure is determined in previous stages, the only thing left to do is to extract the data into that data structure, send it to ODN through Node and automate the whole process.

Steps in the process:
- Generating file from data source to standardized format
- Implementation of procedure that triggers web services that send or receive the files from ODN
- Testing and evaluating the procedures according to use case

Step 7: Utilization of the data on ODN
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After the data is stored on ODN (OriginTrail Decentralised Network), a frontend user interface must access, process and display the data. This step brings the project back to initial use case structuring. This part should be outlined on the beginning of the project as it visualises, what the end beneficiary of the data is seeing. Since each use case needs is unique and requires its own interface, OriginTrail provides API for accessing the data, but does not develop front end solutions (user interface such as mobile app or web site). This is in the domain of the service providers. 

Things needed for consideration when structuring a use case:
- Who is the beneficiary of the data
- How the data will be presented to the data beneficiary
- What kind of user interfaces are needed (web, mobile)



