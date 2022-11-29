## Lab1: Cluster Creation, Data Ingestion and Exploration

This Lab is organized into the following 4 Challenges:
| Challenge | Description | Est. Time |
|--|--|--|
| [Challenge 1](https://github.com/Azure/ADX-in-a-Day-Lab1#challenge-1-create-an-adx-cluster)| Create a free ADX cluster | 10 Min|
| [Challenge 2](https://github.com/Azure/ADX-in-a-Day-Lab1#challenge-2-ingest-data-from-storage-account)| Load Data from Azure Storage| 30 Min|
| [Challenge 3](https://github.com/Azure/ADX-in-a-Day-Lab1#challenge-3-starting-with-the-basics-of-kql)| Starting with the basics of KQL| 1 Hour|
| [Challenge 4](https://github.com/Azure/ADX-in-a-Day-Lab1#challenge-4-explore-and-transform-data)| Explore and Transform Data | 45 min|

Each challenge has a set of tasks that need to be completed in order to move on to the next challenge. It is advisable to complete the challenges and tasks in the prescribed order.

---
Earn a digital badge! In order to receive the "ADX In a Day" digital badge, you will need to complete the tasks marked with 🎓. Please submit the KQL queries/commands of these tasks in the following link: [Answer sheet - ADX Lab 1](https://forms.office.com/r/3V7yjXwAMD)

<img src="/assets/images/Badge_Transparent.png" width="200">

---
---

# [Go to ADX-In-A-Day HomePage](https://github.com/Azure/ADX-in-a-Day)

---
### Challenge 1: Create an ADX cluster
To use Azure Data Explorer (ADX), you first have to create a free ADX cluster, and create one or more databases in that cluster. Each database has tables. Then you can ingest data into a database so that you can run queries against it.

In this Challenge, you will create a Free cluster and a database. You will run simple KQL query in Kusto Web Explorer (KWE UI).

**Expected Learning Outcomes:**
- Create and work with Free ADX cluster.

---
#### Task 1: Create an ADX cluster
Create your free cluster here: https://aka.ms/kustofree

**Note**: The below screenshot is just an example 
![Screen capture 1](/assets/images/onboarding-kusto-free.png)

A Free cluster home page is added to Myclusters pane in UI
![Screen capture 1](/assets/images/Free-cluster-page.png)
  
#### Task 2: Create a database in the free cluster
To create database, once the free cluster is created, you can use the "Create" button in the Create database window 

![Screen capture 1](/assets/images/Free-cluster-create-DB.png)

In the next page, enter the database name you want to use and click 'Next: Create Database"
![Screen capture 1](/assets/images/Free-cluster-create-DB1.png)

  
---
#### Task 3: Write your first Kusto Query Language (KQL) query
  What is a Kusto query?
  Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, that we will use later.<br>
  A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.<br>
  In the next Challenge, we'll ingest data to the cluster, and then learn the most important concepts in KQL and write interesting queries. In this task, you will write a few basic queries to get an understanding of the environment.<br>
  In this example, you'll use the Azure Data Explorer web interface as a query editor (Kusto Query Language can also be used in Azure Monitor Logs, Azure Sentinel, and other services that are built on-top of Azure Data Explorer.)
  
  We can see our cluster and the database that we created.
  To run KQL queries, you must select the query button on the Free Cluster page. <br>
  ![Screen capture 1](/assets/images/Free-cluster-query.png)
  Now – you can write a simple KQL query: print ("hello world"),
  and hit the “Run” button. The query will be executed and its result can be seen in the result grid on the bottom of the page. 
  
  ![Screen capture 1](/assets/images/Free-cluster-helloworld.png)
  
  Windows users can also download [Kusto Explorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer), a desktop client to run queries and benefit from advanced features available in the client.

---
### Challenge 2: Ingest data from Storage Account
  
  Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

  ADX supports several ingestion methods. [These methods include ingestion tools, connectors and plugins, managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.]

  **Expected Learning Outcomes:**
  - Ingest from Storage using .ingest control commands
  - Ingest data using one-click ingestion from Azure Blob Storage(or local files) to your ADX cluster.
  
---
#### Task 1: Create table and ingest data 
Execute the below database script to
  - Create a table - logsRaw
  - Set retention policy to 100 years (Deeper learning in Lab 2 Challenge 6)
  - Load data using .ingest commands from storage account

```
.execute database script with (ContinueOnErrors=true) <|
//
// Create raw table with defined schema
//
.create-merge table logsRaw(Timestamp:datetime, Source:string, Node:string, Level:string, Component:string, ClientRequestId:string, Message:string, Properties:dynamic) 
//
// Retention policy
.alter-merge table logsRaw policy retention softdelete = 36500d recoverability = enabled
//
// Load data- On free cluster, it will take about 20 seconds; 
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/00/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T00:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/01/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T01:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/02/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T02:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/03/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T03:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/04/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T04:00:00Z')
.ingest       into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/05/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T05:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/06/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T06:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/07/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T07:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/08/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T08:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/09/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T09:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/10/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T10:00:00Z')
.ingest       into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/11/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T11:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/12/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T12:00:00Z')
.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/13/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T13:00:00Z')
```

---
#### Task 1: Use the “One-click” UI (User Interfaces) to create a data connection to Azure blob storage
  For the best user experience, we will use the Azure Data Explorer Web UI (aka: Kusto web Explorer/KWE). To open it, click "Query" in Free cluster page by going to [Kusto Web Explorer](https://dataexplorer.azure.com/freecluster).The web UI opens. 
  
  We will ingest one dataset from an Azure Storage account. </br>
  1 csv.gz files are avaialble for download in 'Data' folder in this github page.  </br> 
  
  Go to the “Data management” tab, and select **Ingest data**
  
  ![Screen capture 1](/assets/images/Free-cluster-Ingestdata.png)
  
  Make sure the cluster and the Database fields are correct. Select **New table**
  
  **Note**: We used an example table name as 'logsRaw' here. You can give any name to your table but be sure to use it in all your queries going forward.

  ![Screen capture 1](/assets/images/IngestTable.png)

  **Note**: If you do not have a storage account, follow the "Ingest from File" section below. Otherwise, follow the "Ingest from Storage" section.

  **Ingest from File**: Select "File" as the source type in Ingest data window. Browse your system for the already downloaded Logistics_telemetry_Historical files

  <img src="/assets/images/IngestFromFile.png" width="400">
  
  **Ingest from Storage**: Select "Blob container" as the source type in Ingest data window. In the **Link to source**, paste the SAS URL of the blob container. To get the SAS URL of the blob container, go to this storage account in the Azure portal. Once you're on the storage account page, go to the "Containers" menu and right-click on the container where you uploaded the 3 data files. Click "Generate SAS". A side pane opens. In the "permissions" dropdown, add "list" along with "read". Click "Generate SAS token and URL" and copy the "Blob SAS URL".

  Go back to the ADX “One-click” UI. Paste the SAS URL and select one of the **Schema defining file** that start with "export_" (not all the files in that blob storage have the same schema) and click **Next**
 
  
  ![Screen capture 1](/assets/images/IngestFromStorage.png)
  
  Make sure you use the **CSV Data format**
  
  ![Screen capture 1](/assets/images/IngestFromStorage_schema.png)
  
  Wait for the ingestion to be completed. For production modes, you could use Azure Event Grid for continuous Blob ingestion. The **Event Grid** link under **Continuous Ingestion** will create the Event Grid resource for that. We won't use this option in this Lab.

   <img src="/assets/images/Ingestion-Complete.png" width="520">
  
  Verify that data was ingested to the table. logsRaw table should have 3834012 records

```
  logsRaw
  | count 
```
---
### Challenge 3: Starting with the basics of KQL

In this challenge you’ll write queries in Kusto Query Language (KQL) to explore and gain insights from your data. 

Expected Learning Outcomes:
- Know how to write queries with KQL.
- Use KQL to explore data by using the most common operators.

**What is a Kusto query?**
A Kusto query is a read-only request to process data and return results. The request is stated in plain text that's easy to read, author, and automate. A Kusto query has one or more query statements and returns data in a tabular or graph format.

**What is a tabular statement?**
The most common kind of query statement is a tabular expression statement. Both its input and its output consist of tables or tabular datasets.

Tabular statements contain zero or more operators. Each operator starts with a tabular input and returns a tabular output. Operators are sequenced by a pipe (|). Data flows, or is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step.

It's like a funnel, where you start out with an entire data table. Each time the data passes through another operator, it's filtered, rearranged, or summarized. Because the piping of information from one operator to another is sequential, the query's operator order is important. At the end of the funnel, you're left with a refined output.
Let's look at an example query:

```
logsRaw
| take 10 
```

This query has a single tabular expression statement. The statement begins with a reference to the table githubraw and contains the operators where and count. Each operator is separated by a pipe. The data rows for the source table are filtered by the value of the Type column. In the last line, the query returns a table with a single column and a single row that contains the count of the remaining rows.

References:
- [SQL to Kusto cheat sheet](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)
- [KQL cheat sheets](https://github.com/marcusbakker/KQL/blob/master/kql_cheat_sheet.pdf)

---
#### Task 1: Basic KQL queries - explore the data
  
In this task, you will see some KQL examples. For this task, we will use the table logsRaw, which has data we loaded in previous challenge from storage account. </br> 
Execute the queries and view the results. KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of the data. The where operator filters results that satisfy a certain condition. 

```
logsRaw
| where Level=="Error"
| take 10
```
'take' operator samples any number of records from our table without any order. In the above example, we asked to provide 10 random records.

 Find out how many records are in the table

  ```
logsRaw
| summarize count() // or: count
  ```

Find out the minimum and maximum Timestamp

  ```
logsRaw
| summarize min(Timestamp), max(Timestamp)
 ``` 

KQL makes it simple to access fields in JSON and treat them like an independent column:

```
logsRaw
| where Component == "DOWNLOADER"
| take 100
| extend originalSize=Properties.OriginalSize, compressedSize=Properties.compressedSize
```

---
#### Task 2: Explore the table and columns 🎓
Write a query to get the schema of the table. 
Hint: Observe there are 2 new columns originalSize and compressedSize with datatype 'long'

Example result:  
<img src="/assets/images/Schema.png" width="400">

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)

[getschema operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/getschemaoperator)


---
#### Task 3: Keep the columns of your interest 🎓
Write a query to get only specific desired columns: Timestamp, ClientRequestId, Level, Message. Take arbitrary 10 records.

Example result:</br>
<img src="/assets/images/project.png" width="400">

[project-away operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectawayoperator)

[Project operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)

---
#### Task 4: Filter the output 🎓
Write a query to get only specific desired columns: Timestamp, ClientRequestId, Level, Message. Take arbitrary 10 records. between 2014-03-08 01:00 and 2014-03-08 10:00.

Hint 1: In case you see 0 records, remember that operators are sequenced by a pipe (|). Data is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step. By using the ‘Take’ operator, there is no guarantee which records are returned

[where operator in Kusto query language - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)

---
#### Task 5: Sorting the results 🎓
Write a query to get top 10 records with highest rowcount for INGESTOR_EXECUTER Component field.

Hint: Extract rowCount from Properties column 

[sort operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sortoperator)
[top operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)

---
#### Task 6: Reorder, rename, add columns 🎓
Write a query to extract format and row count from INGESTOR_EXECUTOR component. Rename the field to fileFormat and rowCount respectively. Also, Make Sure Timestamp, fileFormat and rowCount are the first 3 columns

Example result:</br>
<img src="/assets/images/rename_reorder.png" width="600">

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)

[project-rename operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectrenameoperator)

[project-reorder operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectreorderoperator)

---
#### Task 7: Total number of records 🎓
Write a query to find out how many records are in the table by Component. 

[count operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/countoperator)

---
#### Task 8: Aggregations and string operations 🎓
Write a query to find out how many records have 'ingestion' string in Message column. Aggregate the results by Level.

Example result:

<img src="/assets/images/count_by.png" width="250">

[String operators - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)

[summarize operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
#### Task 9: Render a chart 🎓
Write a query to find out how many records are present per Level (aggregated by Level) and render a piechart.

Example result:

<img src="/assets/images/pie.png" width="500">

[render operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)

---
#### Task 10: Create bins and visualize time series 🎓
Write a query to show a timechart of the number of records 30 minute bins (buckets). Each point on the timechart represent the number of logs on that bucket.

Example result:<br>
<img src="/assets/images/timeseries.png" width="650">

[bin() - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)


[summarize operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
#### Task 11: Shortcuts
There are many keyboard shortcuts available in ADX Web UI and Kusto Explorer to increase productivity while working with KQL.
Below are a few examples
- You don't have to select a block of code. Based on current cursor position, code that is separated by empty lines is considered a single block of code.<br>
<img src="/assets/images/codeBlock.png" width="650">

- You can execute a block of code using Shift+Enter
- You can directly insert filters based on data cells selections using Ctrl+Shift+Space <br>
<img src="/assets/images/AddasFilters.png" width="650">

[Kusto Web UI shortcuts | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/web-ui-query-keyboard-shortcuts)

[Kusto Explorer shortcuts | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer-shortcuts)

---
### Challenge 4: Explore and Transform Data

In this challenge we will explore 3 capabilities of Data Explorer

- **Update Policy** is like an internal ETL. It can help you manipulate or enrich the data as it gets ingested into the source table (e.g. extracting JSON into separate columns, creating a new calculated column, joining the new records with a static dimension table that is already in your database, etc). For these cases, using an update policy is a very common and powerful practice.
Each time records get ingested into the source table, the update policy's query (which we'll define in the update policy) will run on them (and only on newly ingested records - other existing records in the source table aren’t visible to the update policy when it runs), and the results of the query will be appended to the target table. This function output schema and target table schema should exactly match.

- **User-defined functions** are reusable subqueries that can be defined as part of the query itself (ad-hoc functions), or persisted as part of the database metadata (stored functions). User-defined functions are invoked through a name, are provided with zero or more input arguments (which can be scalar or tabular), and produce a single value (which can be scalar or tabular) based on the function body.

Expected Learning Outcomes:
- Create user defined functions to use repeatable logic
- Create an update policy to transform the data at ingestion time

For the next task, we will use the githubraw table .

#### Task 1: User defined Function (Stored Functions) 🎓

Create a stored functions that will contain the code of the following logic. Make sure the function works.

Function: Filter records by Ingestion Components -INGESTOR_EXECUTER, INGESTOR_GATEWAY, INTEGRATIONDATABASE,INTEGRATIONSERVICEFLOWS, INTEGRATIONSERVICETRACE, DOWNLOADER


See the [create function](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/functions) article.

---

#### Task 2: Create an update policy 🎓
In this task, we will use an 'update policy' to filter the raw data in the logsRaw table (the source table) for ingestion logs, that will be ingested into new tables that we’ll create (“ingestionLogs”).

**Build the target tables**
```
.create table ingestionLogs (Timestamp: datetime, Source: string, Properties: dynamic, Node: string, Message: string, Level: string, Component: string, ClientRequestId: string)
```
Create a function for the update policy 🎓
 ```
 **Use the function created in Task 1**
```
Create the update policy 🎓
```
     <Complete the command>
```

Update policy can transform and move the data from source table from the time it is created. It cannot look back at already existing data in source table. In order to mimic new data ingesting into source table we will use ".set-or-append" control commnd to ingest 1000 rows into source table (sample data from source table)

```
  .set-or-append logsRaw <| logsRaw | take 1000
```
- [Kusto Ingest from Query | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/management/data-ingestion/ingest-from-query)


Make sure the data is transformed correctly in the destination tables
```
ingestionLogs
| take 10
```

**Relevant docs for this challenge:**
  - [Kusto update policy - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/updatepolicy)

---
---

🎉 Congrats! You've completed ADX in a Day Lab 1. Keep going with [**Lab 2: Advanced KQL, Policies and Visualization**](https://github.com/Azure/ADX-in-a-Day-Lab2)