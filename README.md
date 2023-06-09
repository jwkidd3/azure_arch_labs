# Architect day3:  Data architectures and security
Third day is focusing on data architectures in Azure and data security.

Azure Stencils
* Go to Visio
* Open shapes
* Search for word "cloud" and click for Online results
* Download Microsoft Azure Cloud Icons
* If you have older Visio you can download older stencils directly and copy to Documents/My Shapes folder [here](https://architects.blob.core.windows.net/visio/CnE_CloudV2.7.vss?st=2019-05-20T05%3A27%3A00Z&se=2020-05-21T07%3A27%3A00Z&sp=rl&sv=2018-03-28&sr=b&sig=IELwMsknjeDR3E2fQcfyDa1lOG3hHIiHvLe4gyXMn0U%3D)
* If you are using different tool, download SVG library and import to tool of your choice [here](https://www.microsoft.com/en-us/download/details.aspx?id=41937)

Example solution
- schema https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad
- description https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad#architecture
- pricing https://azure.microsoft.com/en-us/pricing/calculator/
- example architectures [https://azure.microsoft.com/en-us/solutions/architecture/](https://azure.microsoft.com/en-us/solutions/architecture/)

Timing: 
- 10min intro 
- 50min design
- 20min presentation and recommended solution

# Scenarios

## Scenario 1: Design governance solution for secure data in Azure for regulated industry customer

Customer in regulated industry need to store unstructured data in Blob storage and structured data in Azure SQL Database. You need to design solution that is compliant with their security needs.

Here are security requirements:
- All communications (data in fly) must be encrypted
- Team of data analytics professionals need to access Azure SQL database, but for security reasons make sure mutli-factor authentication is supported
- All data (both blobs and SQL) need to be encrypted at rest with customer-managed keys
- SQL database contains one highly sensitive column that needs to be protected by encryption in application so all data is stored already encrypted
- Make sure Azure administrators do not accidentaly replicate data outside of EU
- Design solution in a way, that Storage and SQL accept only communication from VMs in customer VNET
- Make sure that security best practicieas are audited or enforced in automated way
- All audit information needs to be directed to SIEM solution

## Scenario 2: Selecting right storage for different needs and types of data

Company has a lot of different data sources and you need to find best storage option based on data types and needs for additional processing.

Source data:
- Traditional structured dataset with orders and payments with need for strong transaction consistency, thousands of transactions per day. System needs to be ready for future growth in both database size and number of concurrent users. Customer expects huge growth in future.
- Various data files in different formats such as JSON, Excel, CSV, PDF and JPG with different types of information such as maintenance records, audit trails, logs, financial data, scans of invoices, user pictures and avatars. Due to security concerns access to different folders needs to be governed with access control rules.
- JSON files and messages with unpredictable structure with need for indexed queries. Due to need for low latency data must be distributed across 3 different regions to optimize performance for both reads and writes.
- Legal document store with ability to setup WORM (Write Once Read Many - also called immutable storage) for legal hold and lifecycle management to optimize costs of live data vs. archive.

Select ideal Azure solution for each data source. Only use PaaS services to optimize operational costs.

## Scenario 3: Moving data

There are various data sources and locations:
- On-premises FTP server with CSV files
- On-premises SQL Server 2017 with order transactional data in multiple countries
- Telemetry data from IoT sensors collected in IoT Hub
- 3rd party SaaS solution with event data pushed as Kafka messages to endpoint of your choice

Design following solutions:
- Every week move files from FTP server to Azure SQL Database in Azure. To minimize cost and operational overhead use serverless platform (no VMs) and design secure way for communicating from on-premises environment to Azure SQL Database.
- Every night after company stores close orchestrate movement of transactional data from all countries SQL Servers to centralize location in Azure as flat files fo further processing. After all files are gathered from all countries use single job to ingest all data into company data warehouse based on Azure Synapse Analytics.
- Telemetry data needs to be analyzed in real time to identify anomalies and notify application systems. Also all RAW data needs to be stored in Azure Blob Storage for forensic audit purposes while aggregated data in database for reporting.
- Find solution to efficiently and reliably receive Kafka messages and use some processing to convert message format, enrich with static data (numbered list, geo-tagging) and push to Event Hub for further aplication processing (consumer)

## Scenario 4: Designing structured data analysis solution

The company needs to create and maintain data warehouse solution for operational as well as historization purposes. The solution needs to be able to fulfil the following requirements:

- the data is pulled every night from 3 transactional database systems DWH, the DWH solution needs to be more powerful for this load so the process does not last into next day business hours
- for the solution must stay cost effective, so there should be 3 compute level tiers defined on DWH automatically changing according to business hours (8am, 6pm, 10pm for load)
- for operational reporting requirements, every LOB has it's own requirements for data models (which take data from the DWH) these models are defined and automatically refreshed according to LOB business needs
- interactive reports are delivered to the end users via mobile application (tablets, phones) as interactive dashboards (end user is notified when the report has been shared with them or it's data has been updated) 

## Scenario 5: Designing semi-structured and unstructured data analysis solution

Customer wants to design text analylis usecase where the data is captured from the call center calls. You can assume call recordings are offered as audio files by agency. Suggest the solution how to:

- exchange the audio files with external agency for further processing
- convert the audio in to text - this text needs to be stored somewhere for further processing in desired language
- analyze the text for sentiment as well as extract the key phrases from it & store it somewhere so you can analyze them
- enable full text search within the text as well as through key phrases extracted in cost effective whilst performant mannner 

## Scenario 6: Designing Machine Learning

Assume you have e-commerce platform offering multiple product groups spanning from mobile phones, home electronics to workshop tools. Suggest the platform that enables data scientists and data engineers to achieve the following:

- create recommendation engine machine learning models to be able to recommend customer next product according to their online web shop behaviour as well as previous orders (assume you have this data as an input)
- be able to create models coded in Python with the option to choose from multiple compute options where your model could be trained
- evaluate multiple version of your machine learning models with assessing their quality and performance - the comparison could be done programatically or visually
- the environment needs to support multiple data scientists to be working on the same project with version control included in the solution
- have an option how move the "right" model into production so the current e-commerce platform is able to use it as the scoring function against new data coming in


## Contacts

