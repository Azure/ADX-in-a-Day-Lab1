# **Lab1: Cluster Creation, Data Ingestion and Exploration**

This Lab is organized into the following 4 Challenges:
| Challenge | Description | Est. Time |
|--|--|--|
| [Challenge 1](#challenge-1-create-an-adx-cluster)| Create a free ADX cluster | 15 Min|
| [Challenge 2](#challenge-2-ingest-data-from-azure-storage-account)| Load Data from Azure Storage| 30 Min|
| [Challenge 3](#challenge-3-starting-with-the-basics-of-kql)| Starting with the basics of KQL| 1 Hour|
| [Challenge 4](#challenge-4-explore-and-transform-data)| Explore and Transform Data | 45 min|

Each challenge has a set of tasks that need to be completed in order to move on to the next challenge. It is advisable to complete the challenges and tasks in the prescribed order.

---
## **Earn a digital badge!** 
In order to receive the "ADX-In-A-Day" digital badge, you will need to complete the tasks marked with ✅ in Lab 1 & Lab 2. Submit your answers for Lab 1 and Lab 2 quizzes in order to receive the "ADX in a Day" digital badge. You may edit your answers after or try again. 

> **For Lab 1, please submit the results for the tasks marked with ✅ in the following link**: [Quiz ADX in A Day Lab 1](https://forms.office.com/r/qZ0yghDwyb)

> **Please allow us 5 working days to issue the badge**

<p align="center"><img src="/assets/images/badge.png" width="200"></p>

---
# [Go to ADX-In-A-Day HomePage](https://github.com/Azure/ADX-in-a-Day)
---
## **Challenge 1: Create an ADX cluster**
To use Azure Data Explorer (ADX), you first have to create a free ADX cluster, and create one or more databases in that cluster. Each database has tables. Then you can ingest data into a database so that you can run queries against it.

In this Challenge, you will create a Free cluster and a database. You will run simple KQL query in Kusto Web Explorer (KWE UI).

**Challanges:**
- [Challenge 1, Task 1: Create an ADX cluster and Database](#challenge-1-task-1-create-an-adx-cluster-and-database)
- [Challenge 1, Task 2: Review the free cluster home page and the Azure Data Explorer Web UI](#challenge-1-task-2-review-the-free-cluster-home-page-and-the-azure-data-explorer-web-ui)
- [Challenge 1, Task 3: Write your first Kusto Query Language (KQL) query](#challenge-1-task-3-write-your-first-kusto-query-language-kql-query)

**Expected Learning Outcomes:**
- Create and work with Free ADX cluster.

---
### **Challenge 1, Task 1: Create an ADX cluster and Database**
1. Create your free cluster and database here: [Free ADX Cluster](https://aka.ms/kustofree). If you want to open the ADX Web UI in another Tab click on the link holding down the ``CTRL`` Key.

    ![Screen capture 1](/assets/images/CreateNewCluster.png)
  
### **Challenge 1, Task 2: Review the free cluster home page and the Azure Data Explorer Web UI**

1. Have a look at the cluster home page.

2. Click on the Icon **My Cluster** in the left navigation pane. 

    ![Screen capture 1](/assets/images/free_cluster_create_db.png)


    On the page **My Cluster**, you'll see the following:
    * Your cluster's name, the option to upgrade to a full cluster, and the option to delete the cluster.
    * Cluster details like: cluster's location, and URI links for connecting to your cluster via APIs or other tools.
    * Quick actions you can take to get started with your cluster.
    * A list of databases in your cluster.
    
3. Click on the Button **Create** in the tile **Create Database** 

4. Enter a name for the database in field **Database**. As an example for the name you can use ``FreeTestDB``.

> **NOTE:** If you already have a free cluster and just want to create a new database for this lab, use the **Create** button in the Create database tile.
  
### **Challenge 1, Task 3: Write your first Kusto Query Language (KQL) query**
  
***What is a Kusto query?*** 

Azure Data Explorer provides a web experience that enables you to connect to your Azure Data Explorer clusters and write and run Kusto Query Language queries. The web experience is available in the Azure portal and as a stand-alone web application, the Azure Data Explorer Web UI, which we will use later.

A *Kusto query* is a read-only request to process data and return results. The request is stated in plain text that's easy to read. A Kusto query has one or more query statements and returns data in a tabular or graph format.

In the next Challenge, we'll ingest data to the cluster, and then learn the most important concepts in KQL and write interesting queries. In this task, you will write a basic query to get an understanding of the environment.

In this example, you'll use the Azure Data Explorer web interface as a query editor. 

Kusto Query Language can also be used in other services that are built on-top of Azure Data Explorer, like: 
- [Azure Monitor Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/data-platform-logs)
- [Azure Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/overview)
- [Microsoft Defender for IoT](https://www.microsoft.com/en-us/security/business/endpoint-security/microsoft-defender-iot) 
- [Microsoft Defender for Endpoint](https://www.microsoft.com/en-us/security/business/endpoint-security/microsoft-defender-endpoint)
- [Microsoft Defender for Cloud](https://www.microsoft.com/en-us/security/business/cloud-security/microsoft-defender-cloud)
- [Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview?tabs=net)
  
1. We can see our cluster and the database that we created. If you followd the steps above the database is named ``FreeTestDB``.
  
2. To run KQL queries, you must select the **Query** button on the Free Cluster page. <br>

    ![Screen capture 1](/assets/images/free_cluster_query.png)
  
  3. Now you can write a simple KQL query: 

      ```
        print "Hello World"
      ```
  4. Hit the **Run** button. The query will be executed and its result can be seen in the result grid at the bottom of the page. 
  
      ![Screen capture 1](/assets/images/hello_world.png)
  
  > Windows users can also download [**Kusto Explorer**](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer), a desktop client to run the queries and benefit from advanced features available in the client.

## **Challenge 2: Ingest data from Azure Storage Account**
  
Data ingestion to ADX is the process used to load data records from one or more sources into a table in your ADX cluster. Once ingested, the data becomes available for query.

ADX supports several ingestion methods, including ingestion tools, connectors and plugins, Azure managed pipelines, programmatic ingestion using SDKs, and direct access to ingestion.

**Challanges**
- [Challenge 2, Task 1: Create the raw table - logsRaw](#challenge-2-ingest-data-from-azure-storage-account)
- [Challenge 2, Task 2: Use the “One-click” UI (User Interface) to ingest data from Azure blob storage](#challenge-2-task-2-use-the-one-click-ui-user-interface-to-ingest-data-from-azure-blob-storage)

**Expected Learning Outcomes:**
  - Ingest data using one-click ingestion from Azure Blob Storage to your ADX cluster.
  
---
### **Challenge 2, Task 1: Create the raw table - logsRaw**
1. Go to the **Query** tab and run the following command to create our table:
    ```
    .create table logsRaw(
        Timestamp:datetime, 
        Source:string, 
        Node:string, 
        Level:string, 
        Component:string, 
        ClientRequestId:string, 
        Message:string, 
        Properties:dynamic
    ) 
    ```
### **Challenge 2, Task 2: Use the “One-click” UI (User Interface) to ingest data from Azure blob storage**
You need to analyze the system logs for Contoso, which are stored in Azure blob storage.

1. Go back to the **My Cluster** page, click the **Ingest** button in the tile **Ingest Data**.
  
      ![Screen capture 1](/assets/images/data_ingest.png)

2. Make sure the cluster and the Database fields are correct. In our example the cluster is named ``MyFreeCluster`` and the database is named ``ADX in a day``. Select the option **Existing table**.

      ![Screen capture 1](/assets/images/ingest_table.png)
  
    > **Note**: We used an example table name as ``logsRaw`` here. You can give any name to your table but be sure to use it in all your queries going forward.
  
3. Ingest from Storage:
Select **Blob container** as the **Source type** in the **Source** tab. As **Ingestion type** you can leave the default selection **One-Time**. For **Select source** you can use the default value **Add URL** because we will add a SAS Url next.

4. In the **Link to source**, paste the following SAS ([*Shared Access Signature*](https://learn.microsoft.com/en-us/shows/inside-azure-for-it/introduction-to-sas-shared-access-signature)) URL of the blob storage. SAS URL is a way to provide limited, time-bound access to Azure storage resources such as Blobs. 
    ```
    https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D
    ``` 

5. In the list **Schema defining file** select a file. This file is used to determine the schema of the data. One file is autoselected unless you want to change that. In our example it does not matter which file you choose because all files have the same structure, so you can stick with the autoselected file and click **Next: Schema**.

    ![Screen capture 1](/assets/images/ingest_from_storage.png)


6. Under Data format, make sure you select **Keep current table schema** and deselect **Ignore the first record**. Click on **Next: Start ingestion**.
  
    ![Screen capture 1](/assets/images/ingest_from_storage_schema.png)
  
7. Wait for the ingestion to be completed, and click **Close**.

    ![Screen capture 1](/assets/images/ingestion_completed.png)
  
8. Go to the **Query** page. Run the following KQL query to verify that data was ingested to the table. 
    ```
      logsRaw
      | count 
    ```

    The ``logsRaw`` table should have 3,834,012 records

---
## **Challenge 3: Starting with the basics of KQL**

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

This query has a single tabular expression statement. The statement begins with a reference to the table logsRaw and contains the operators take. Each operator is separated by a pipe.

References:
- [KQL cheat sheets](https://learn.microsoft.com/en-us/azure/data-explorer/kql-quick-reference)

---
### **Challenge 3, Task 0 : Journey from SQL to KQL!**
For all the SQL pros out there, Azure data explorer allows a subset of TSQL queries. Try running the following SQL query in web UI
```
select count() from logsRaw
```
**Note**: Intellisense will not work for SQL queries.

The primary language to interact with Kusto is KQL (Kusto Query Language). To make the transition and learning experience easier, you can use 'explain' operator to translate SQL queries to KQL.

```
explain select max(Timestamp) as MaxTimestamp from logsRaw where Level='Error'
```
Output of the above query will be a corresponsing KQL query
```
logsRaw
| where (Level == "Error")
| summarize MaxTimestamp=max(Timestamp)
| project MaxTimestamp
```
References:
- [SQL to KQL cheat sheets - aka.ms/SQL2KQL](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)
---
### **Challenge 3, Task 1: Basic KQL queries - explore the data**
  
In this task, you will see some KQL examples. For this task, we will use the table logsRaw, which has data we loaded in previous challenge from storage account. </br> 
Execute the queries and view the results. KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of data. <br>
The where operator filters results that satisfy a certain condition. 

```
logsRaw
| where Level=="Error"
| take 10
```
The 'take' operator samples any number of records from our table without any order. In the above example, we asked to provide 10 random records.

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
 
Azure Data Explorer provides a set of system data types that define all the types of data that can be stored. <br>
Some data types for example are: string, int, decimal, GUID, bool, datetime. <br><br>
Note, that the data type of the *Properties* column is *dynamic*. The *dynamic* data type is special in that it can take on any value of other data types, as well as arrays and property bags (dictionaries). <br>
Our dataset has trace records written by Contoso's DOWNLOADER program (*| where Component == "DOWNLOADER"*), which downloads files from blob storage as part of its business operations. This is how a typical *Properties* column looks like:
<img src="/assets/images/properties_column.png" width="1200">

The *dynamic* type is extremely beneficial when it comes to storing JSON data, since KQL makes it simple to access fields in JSON and treat them like an independent column: just use either the dot notation (*dict.key*) or the bracket notation (*dict["key"]*). <br>

The *extend* operator adds a new calculated column to the result set, during query time. This allows for the creation of new standalone columns to the result set, from the JSON data in *dynamic* columns.

```
logsRaw
| where Component == "DOWNLOADER"
| take 100
| extend originalSize=Properties.OriginalSize, compressedSize=Properties.compressedSize
```

Note that although the dynamic type appears JSON-like, it can hold values that the JSON model does not represent because they don't exist in JSON (e.g., long, real, datetime, timespan, and GUID). 

---
### **Challenge 3, Task 2: Explore the table and columns ✅**
After subscripting a dynamic object, it is necessary to cast (convert) the value to a simple type in order to utilize them (for example, if you want to summarize the sizes of all the *OriginalSize*, you should convert the *dynamic* type to a numeric type, like *long*).<br><br>

Write a query to get the table that is shown in the image below (we want to convert the *OriginalSize* and *CompressedSize* columns to *long*).
<br><br>
Hint 1: Observe there are 2 new columns originalSize and compressedSize with datatype 'long' <br>
Hint 2: Accessing a sub-object of a dynamic value yields another dynamic value, even if the sub-object has a different underlying type. <br>
Hint 3: After subscripting a dynamic object, you must cast the value to a simple type.

**Question**: What is the "Datatype" of "ColumnType = long ?

Example result:  
![Screen capture 1](/assets/images/get_schema.png)

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)

[tolong()](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/tolongfunction)

[getschema operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/getschemaoperator)

---
### **Challenge 3, Task 3: Keep the columns of your interest ✅**
You are investigating an incident and wish to review only several columns of the dataset. <br>
Write a query to get only specific desired columns: Timestamp, ClientRequestId, Level, Message. Take arbitrary 10 records.

**Question**: If we have to change ClientRequestId column from string to guid datatype, what is the function we should use?

Example result:
![Screen capture 1](/assets/images/project.png)

[project operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)

---
### **Challenge 3, Task 4: Filter the output ✅**
You are investigating an incident that occurred within a specific time frame. <br> Write a query to get only specific desired columns: Timestamp, ClientRequestId, Level, Message. Take all the records between 2014-03-08 01:00 and 2014-03-08 10:00.

Hint 1: In case you see 0 records, remember that operators are sequenced by a pipe (|). Data is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step. By using the ‘Take’ operator, there is no guarantee which records are returned

**Question**: What is the count of records in this timeframe?

[datetime data type](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/scalar-data-types/datetime)<br>
[where operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)<br>
[between operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/betweenoperator#filter-datetime)

---
### **Challenge 3, Task 5: Sorting the results ✅**
Your system generated an alert indicating a significant decrease in incoming data. You want to check the traces of the "INGESTOR_EXECUTER" [sic] component of the program. <br>
Write a query that returns 20 sample records in which the *Component* column equals the word "INGESTOR_EXECUTER" [sic].
Once done, rewrite the query to take the top 1 records by the value of *rowCount* (for the "INGESTOR_EXECUTER" [sic] records).

Hint 1: Extract rowCount from Properties column

Hint 2: Think about the datatype and conversion

Hint 3: Note the "Executer" [sic] spelling

**Question**: What is the value of rowCount column of this record?

Example result:</br>
<img src="/assets/images/top_10_rowCount.png" width="950">

[sort operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sort-operator)
[top operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)

---
### **Challenge 3, Task 6: Data profiling ✅**
As part of the incident investigation, you want to extract *format* and *rowCount* from INGESTOR_EXECUTER [sic] component. Rename the calculated fields to fileFormat and rowCount respectively. Also, Make Sure *Timestamp*, *fileFormat* and *rowCount* are the first 3 columns. 

**Question**: How many different file formats are present in this data?

Example result:</br>
<img src="/assets/images/rename_reorder.png" width="950">

[distinct operator](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/distinctoperator)

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)

[project-rename operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectrenameoperator)

[project-reorder operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectreorderoperator)

---
### **Challenge 3, Task 7: Total number of records ✅**
The system has several components, but you don't know what they are. 
The system comprises of several "components", but you don't know their names or how many records were generated by each.<br>
Write a query to find out how many records were generated by each component. Use the *Component* column. 

**Question**: What is the count of "DATAACCESS" component?

[count aggregation function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/count-aggfunction)

---
### **Challenge 3, Task 8: Aggregations and string operations ✅**
You assume that the incident being investigated has a connection to the ingestion process run by Contoso's program.<br>
Write a query to find out how many records contain the string 'ingestion' in the *Message* column. Aggregate the results by *Level*.

**Question**: What is count of "Error" records?

Example result:

<img src="/assets/images/count_by.png" width="300">

[String operators](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)

[summarize operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
### **Challenge 3, Task 9: Render a chart ✅**
Write a query to find out how many total records are present per *Level* (aggregated by *Level*) and render a piechart.

**Question**: What is the "Warning" %?

Example result:

<img src="/assets/images/pie.png" width="500">

[render operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)

---
### **Challenge 3, Task 10: Create bins and visualize time series ✅**
Write a query to show a timechart of the number of records in 30 minute bins (buckets). Each point on the timechart represent the number of logs in that bucket.

**Question**: What is the count of records on March 8th, 10:30 ?

Example result:<br>
<img src="/assets/images/timeseries.png" width="500">

[bin() - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)


[summarize operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
### **Challenge 3, Task 11: Shortcuts**

Purpose of this task is to expose some cool productivity features of Azure Data Explorer Web Interface. This task is not evaluated.

There are many keyboard shortcuts available in ADX Web UI and Kusto Explorer to increase productivity while working with KQL.
Below are a few examples
- You don't have to select a block of code. Based on current cursor position, code that is separated by empty lines is considered a single block of code.<br>
<img src="/assets/images/code_block.png" width="500">

- You can execute a block of code using Shift+Enter
- You can directly insert filters based on data cells selections using Ctrl+Shift+Space <br>

![Screen capture 1](/assets/images/add_as_filters.gif)

[Kusto Web UI shortcuts | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/web-ui-query-keyboard-shortcuts)

[Kusto Explorer shortcuts | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer-shortcuts)

---
## **Challenge 4: Explore and Transform Data**

In this challenge we will explore 3 capabilities of Data Explorer

- **Update Policy** is like an internal ETL. It can help you manipulate or enrich the data as it gets ingested into the source table (e.g. extracting JSON into separate columns, creating a new calculated column, joining the newly ingested records with a static dimension table that is already in your database, etc). For these cases, using an update policy is a very common and powerful practice.<br>
Each time records get ingested into the source table, the update policy's query (which we'll define in the update policy) will run on them (and **only on newly ingested records** - other existing records in the source table aren’t visible to the update policy when it runs), and the results of the query will be appended to the target table. This function's output schema and target table schema should exactly match.

- **User-defined functions** are reusable KQL subqueries that can be defined as part of the query itself (ad-hoc functions), or persisted as part of the database metadata (stored functions - reusable KQL query, with the given name). Stored functions are invoked through a name, are provided with zero or more input arguments (which can be scalar or tabular), and produce a single value (which can be scalar or tabular) based on the function body.

Expected Learning Outcomes:
- Create user defined functions to use repeatable logic
- Create an update policy to transform the data at ingestion time

<img src="/assets/images/Update_policy.png" width="500">

For the next task, we will use the *logsRaw* table.

---

### **Challenge 4, Task 1: User defined Function (Stored Functions) ✅**

Create a stored functions, named *ManiputatelogsRaw*, that will contain the code below. Make sure the function works.

 ```
logsRaw
| where Component in ('INGESTOR_EXECUTER', 'INGESTOR_GATEWAY', 'INTEGRATIONDATABASE','INTEGRATIONSERVICEFLOWS', 'INTEGRATIONSERVICETRACE', 'DOWNLOADER')
 ```
**Question**: What is property used when creating function that is used for UI functions categorization?

See the [create function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/management/create-function) article.

---

### **Challenge 4, Task 2: Create an update policy ✅**
In this task, we will use an 'update policy' to filter the raw data in the logsRaw table (the source table) for ingestion logs, that will be ingested into new tables that we’ll create (“ingestionLogs”).

**Build the target tables**
```
.create table ingestionLogs (Timestamp: datetime, Source: string,Node: string, Level: string, Component: string, ClientRequestId: string, Message: string, Properties: dynamic)
```
Create a function for the update policy 
 ```
 **Use the function created in Task 1**
```

Create the update policy(Fill in the blanks) ✅
```
.alter table ...... policy update 
@'[{ "IsEnabled": true, "Source": "....", "Query": ".....", "IsTransactional": true, "PropagateIngestionProperties": false}]'
```
 [Kusto update policy - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/updatepolicy)
 
Update policy can transform and move the data from source table from the time it is created. It cannot look back at already existing data in source table. We will ingest new data into logsraw table and see new data flowing into ingestionLogs table

```
// Note: execute the below commands one after another => Using operationId(output of each command), check the status and execute a new command only after the previous one is completed

.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/00/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T00:00:00Z');

.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/01/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T01:00:00Z');

.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/02/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T02:00:00Z');

.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/03/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T03:00:00Z');

.ingest async into table logsRaw (h'https://logsbenchmark00.blob.core.windows.net/logsbenchmark-onegb/2014/03/08/04/data.csv.gz?sp=rl&st=2022-08-18T00:00:00Z&se=2030-01-01T00:00:00Z&spr=https&sv=2021-06-08&sr=c&sig=5pjOow5An3%2BTs5mZ%2FyosJBPtDvV7%2FXfDO8pLEeeylVc%3D') with (format='csv', creationTime='2014-03-08T04:00:00Z');
```
[Kusto Ingest from Storage | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/management/data-ingestion/ingest-from-storage)

**Note:** The above command does not complete immediately. Because we used the 'async' parameter, the output of the above query will be operationIds. The progress of the query can be checked by used the below command
```
  .show operations <operationId>
```

Make sure the data is transformed correctly in the destination tables
```
ingestionLogs
| count
```
- Check if the count of ingestionLogs table is 93648.

**Note:** If the count is not matching for ingestionLogs table, it means that one of the above .ingest commands have throttled or failed. Please run the following command to clean ingestionLogs table
```
.clear table ingestionLogs data
```
You can then run the above .ingest commands one by one and this will result in 93648 count in ingestionLogs table.

Hint 1: Remember we have already ingested data into logsRaw in Challenge 2. We need the count of records from latest ingestion only. 

Hint 2: ingestion_time() is a hidden column that stores ingested datetime of extents. 

Hint 3: Use [ago()](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/agofunction) to filter for latest records inserted.

- What is the count of records that were ingested into ingestionLogs(target) table with this update policy?

**Question**: Calculate the ratio => ingestionLogs count / logsRaw count (Only the latest ingestion). Consider 4 digits after decimal points in the output.

---
---

🎉 Congrats! You've completed ADX in a Day Lab 1. Keep going with [**Lab 2: Advanced KQL, Policies and Visualization**](https://github.com/Azure/ADX-in-a-Day-Lab2)
