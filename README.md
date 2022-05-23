## Azure Synapse Link For SqlDB 1-Click Environment
Azure Synapse Link for SQL enables near real time analytics over operational data in Azure SQL Database or SQL Server 2022. With a seamless integration between operational stores including Azure SQL Database and SQL Server 2022 and Azure Synapse Analytics, Azure Synapse Link for SQL enables you to run analytics, business intelligence and machine learning scenarios on your operational data with minimum impact on source databases with a new change feed technology.

This 1-click deployment allows the user to deploy an environment with Synapse and Azure SqlDB, you can directly access SqlDB tables from Azure Synapse Analytics workspace. This package provides complete scenario to setup "Azure SQLDB with sample dataset" and "Setup Synapse link for SqlDB".


![SynapseSqlDB](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/synapse-link-sql-architecture.png)

## Prerequisites

Owner role (or Contributor roles) for the Azure Subscription the template being deployed in. This is for creation of a separate Proof-of-Concept Resource Group and to delegate roles necessary for this proof of concept. Refer to this [official documentation](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-steps) for RBAC role-assignments.

## Deployment Steps
    
1. Click 'Deploy To Azure' button given below to deploy all the resources.

    [![Deploy To Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.svg?sanitize=true)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2FTest-Drive-Synapse-Link-For-SqlDB-With-1-Click%2Fmain%2Fazuredeploy.json)

   - Provide the values for:

     - Resource group (create new)
     - Region
     - Company Tla - *Any three Characters to keep the Azure resource names unique*
     - Option (true or false) for Allow All Connections - *Firewall to allow/deny connection to Synapse SQL*
     - Option (true or false) for Spark Deployment - *Synapse Spark pool deployment*
     - Spark Node Size *(Small, Medium, large) if Spark Deployment is set to true*
     - Sql Administrator Login
     - Sql Administrator Login Password
     - Sku - *SKU of the Synapse SQL pool*
     - Option (true or false) for Metadata Sync - *Serverless SQL pool can automatically synchronize metadata from Apache Spark*
     - Frequency - *Choose whether to run schedule every day of the week, or only on weekdays*
     - Time Zone
     - Resume Time - *Time of Day when the data warehouse will be resumed*
     - Pause Time - *Time of day when the data warehouse will be paused*
     - SQLDBDeployment - *To deploy an Azure SQLDB with Sample Dataset*

   - Click 'Review + Create'.
   - On successful validation, click 'Create'.

## Azure Services being deployed
This template deploys necessary resources to run an Azure Synapse link for SQlDB. 
Following resources are deployed with this template along with some RBAC role assignments:

- An Azure Synapse Workspace 
- An Azure Synapse SQL Pool
- Azure SQLDB (Optional)
- Azure SQL Server (Optional)
- Apache Spark Pool (Optional)
- Azure Data Lake Storage Gen2 account
- A new File System inside the Storage Account to be used by Azure Synapse
- A Logic App to Pause the SQL Pool at defined schedule
- A Logic App to Resume the SQL Pool at defined schedule
- A key vault to store the secrets


## Post Deployment
- If you are configuring Azure Synapse link for on-prem SQL Server, [Follow instructions here](https://review.docs.microsoft.com/en-us/azure/synapse-analytics/synapse-link/connect-synapse-link-sql-server-2022?branch=release-build-synapse-link-sql)
- After the deployment is complete, click 'Go to resource group'.
- You'll see all the resources deployed in the resource group.
- Click on the newly deployed Synapse workspace.
- Click on link 'Open' inside the box labelled as 'Open Synapse Studio'.




![PostDeployment-1](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/PostDeployment_1.gif)

## Setup Azure Synapse Link connection
- Open the Integrate hub, and select + Link connection(Preview).
- Under Source linked service, select New.
- Select the information for your source Azure SQL Database already deployed as part of this package.
- Select the subscription, server, and database corresponding to your Azure SQL Database
- Use SQL Authentication username/password(Created while deploying this package).
- Keyvault secerts can be also used deployed as part of this package.
- Select Test connection to ensure the firewall rules are properly configured and the workspace can successfully connect to the source Azure SQL Database.
- Select Create.

![SetupSQL-Link](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/SetupSQLLink.gif)

## Select SQL DB Tables for Azure Synapse Link connection
- Select one or more source tables to replicate to your Synapse workspace and select Continue.
- Select a target Synapse SQL database and pool.
- Provide a name for your Azure Synapse Link connection, and select the number of cores. These cores will be used for the movement of data from the source to the         target.
- Select OK.
- Click 'Publish'. A blade will appear from right side of the window.

![Publish-SQL-Link](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/PublishSQLLink.gif)

## Start the Azure Synapse Link connection
- Select Start and wait a few minutes for the data to be replicated.
- You may monitor the status of your Azure Synapse Link connection, see which tables are being initially copied over (Snapshotting), and see which tables are in         continuous replication mode (Replicating).

![StartSQL-Link](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/StartSQLLink.gif)

## Query Replicated SQLDB Data

- In the Data hub, under Workspace, open your target database, and within Tables, right-click one of your target tables.
- Choose New SQL script, then Select TOP 100 rows.
- Run this query to view the replicated data in your target Synapse SQL database.

![QuerySQLTables-Link](https://github.com/azure/Test-Drive-Synapse-Link-For-SqlDB-With-1-Click/blob/main/images/QuerySQLDBTables.gif)
