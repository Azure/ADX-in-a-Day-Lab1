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
---

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
  - Ingest data using one-click ingestion from Azure Blob Storage(or local files) to your ADX cluster.

---
#### Task 1: Use the “One-click” UI (User Interfaces) to create a data connection to Azure blob storage
  For the best user experience, we will use the Azure Data Explorer Web UI (aka: Kusto web Explorer/KWE). To open it, click "Query" in Free cluster page by going to [Kusto Web Explorer](https://dataexplorer.azure.com/freecluster).The web UI opens.
  
    
   For this Lab,we use messages that are in JSON format. This is how a sample message looks like:
  ```
  {
  "messageProperties": {
    "iothub-creation-time-utc": "2021-12-22T11:16:58.668Z"
  },
  "enrichments": {},
  "applicationId": "1b2f5f29-a78b-4012-bf31-2016473cadf6",
  "deviceId": "13n5b9yiael",
  "messageSource": "telemetry",
  "telemetry": {
    "Status": "Online",
    "BatteryLife": 52,
    "Light": 62602.66864777621,
    "Tilt": 44.70275959833819,
    "Shock": -6.381560743718394,
    "ActiveTags": 164,
    "Location": {
      "lon": -68.4585,
      "lat": 40.9633,
      "alt": 1069.4617
    },
    "TransportationMode": "Train",
    "LostTags": 5,
    "Temp": 13.178830710467722,
    "Humidity": 91.17280445807984,
    "Pressure": 1033.3527307505506,
    "TotalTags": 187
  },
  "schema": "default@v1",
  "enqueuedTime": "2021-12-22T11:16:58.753Z",
  "templateId": "dtmi:ltifbs50b:mecybcwqm"
}
```
  
  We will ingest data one dataset from an Azure Storage account. </br>
    1. Logistics telemetry data. The table will be named LogisticsTelemetryHistorical.  </br> 
  
  Go to the “Data management” tab, and select **Ingest data**
  
  ![Screen capture 1](/assets/images/Free-cluster-Ingestdata.png)
  
  Make sure the cluster and the Database fields are correct. Select **New table**
  
  <img src="/assets/images/Challenge2-Task3-Pic2.png" width="450">

  **Note**: If you do not have a storage account, follow the "Ingest from File" section below. Otherwise, follow the "Ingest from Storage" section.

  **Ingest from File**: Select "File" as the source type in Ingest data window. Browse your system for the already downloaded Logistics_telemetry_Historical files

  <img src="/assets/images/Free-cluster-ingestfromFile.png" width="400">
  
  **Ingest from Storage**: Select "Blob container" as the source type in Ingest data window. In the **Link to source**, paste the SAS URL of the blob container. As a part of pre-requisites, we uploaded 3 Logistics_telemetry_Historical files to a storage account. To get the SAS URL of the blob container, go to this storage account in the Azure portal. Once you're on the storage account page, go to the "Containers" menu and right-click on the container named "data". Click "Generate SAS". A side pane opens. In the "permissions" dropdown, add "list" along with "read". Click "Generate SAS token and URL" and copy the "Blob SAS URL".

  Go back to the ADX “One-click” UI. Paste the SAS URL and select one of the **Schema defining file** that start with "export_" (not all the files in that blob storage have the same schema) and click **Next**
 
  
  ![Screen capture 1](/assets/images/Challenge2-Task3-Pic3.png)
  
  Make sure you use the **JSON Data format**
  
  ![Screen capture 1](/assets/images/Challenge2-Task3-Pic4.png)
  
  Wait for the ingestion to be completed. For production modes, you could use Azure Event Grid for continuous Blob ingestion. The **Event Grid** link under **Continuous Ingestion** will create the Event Grid resource for that. We won't use this option in this Lab.

   <img src="/assets/images/ingestion-completed.png" width="520">
  
  Verify that data was ingested to the table

```
  LogisticsTelemetryHistorical
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
LogisticsTelemetryHistorical
| where enqueuedTime > ago(7d) 
| where messageSource == "telemetry"
| count 
```

This query has a single tabular expression statement. The statement begins with a reference to the table LogisticsTelemetry and contains the operators where and count. Each operator is separated by a pipe. The data rows for the source table are filtered by the value of the enqueuedTime column and then filtered by the value of the messageSource column. In the last line, the query returns a table with a single column and a single row that contains the count of the remaining rows.

References:
- [SQL to Kusto cheat sheet](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sqlcheatsheet)
- [KQL cheat sheets](https://github.com/marcusbakker/KQL/blob/master/kql_cheat_sheet.pdf)

<!--
#### Task 0: Connect to the cluster

For the following tasks, connect to the cluster [ADX Lab Cluster](https://adxLabcluster.eastus.kusto.windows.net/)

![Screen capture 1](/assets/images/task5-Task0-Pic1.png)

![Screen capture 1](/assets/images/task5-Task0-Pic2.png)

-->

---
#### Task 1: Basic KQL queries - explore the data
  
In this task, you will see some KQL examples. For this task, we will use the table LogisticsTelemetryHistorical, which has data we loaded in previous challenge from storage account. </br> 
Execute the queries and view the results. KQL queries can be used to filter data and return specific information. Now, you'll learn how to choose specific rows of the data. The where operator filters results that satisfy a certain condition. 

For the following tasks, we will use the table LogisticsTelemetryHistorical.

```
LogisticsTelemetryHistorical
| where deviceId startswith "x"
| take 10
```

Similarly, you can filter where the time of an event occurred more than a certain number of years/days/minutes ago. For example, run the following query, where 2m means 2 minutes:

  ```
LogisticsTelemetryHistorical
| where enqueuedTime > ago(2m)
| take 10 
  ```

Find out how many records are in the table

  ```
LogisticsTelemetryHistorical
| summarize count() // or: count
  ```

Find out how many records have enqueuedTime bigger than the last 10 minutes.

  ```
LogisticsTelemetryHistorical
| where enqueuedTime > ago(10m)
| summarize count()
 ``` 
 
Find out how many records have deviceId that startswith "x" 

```
LogisticsTelemetryHistorical
| where deviceId startswith "x"
| summarize count()
```
  
Find out how many records have deviceId that startswith "x", per device ID (aggregate by device ID)

```
LogisticsTelemetryHistorical
| where deviceId startswith "x"
| summarize count() by deviceId
```
  
Find out how many records startswith "x", per device ID (aggregate by device ID). Render a piechart

```
LogisticsTelemetryHistorical
| where deviceId startswith "x"
| summarize count() by deviceId
| render piechart 
```

KQL makes it simple to access fields in JSON and treat them like an independent column:

```
LogisticsTelemetryHistorical
// | where enqueuedTime > ago(10d) 
| extend h = telemetry.Humidity
| summarize avg(toint(h)) by bin(enqueuedTime, 1h)
| render timechart 
```

---
#### Task 2: Explore the table and columns 🎓
Write a query to get the schema of the table. 

Hint 1: Observe that new columns like Shock, Temp are extracted from original message.

Expected result:  
<img src="/assets/images/Schema.png" width="400">

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)

[getschema operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/getschemaoperator)


---
#### Task 3: Keep the columns of your interest 🎓
Write a query to get only specific desired columns: deviceId, enqueuedTime, Temp. Take arbitrary 10 records.

Expected result:</br>
<img src="/assets/images/project.png" width="400">

[project-away operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectawayoperator)

[Project operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectoperator)

---
#### Task 4: Filter the output 🎓
Write a query to get only specific desired columns: deviceId, enqueuedTime, Temp. Take arbitrary 10 records from the past 90 days.

Hint 1: “ago” </br>
Hint 2: In case you see 0 records, remember that operators are sequenced by a pipe (|). Data is piped, from one operator to the next. The data is filtered or manipulated at each step and then fed into the following step. By using the ‘Take’ operator, there is no guarantee which records are returned

[where operator in Kusto query language - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/whereoperator)

---
#### Task 5: Sorting the results 🎓
Write a query to get the 5 records which have the highest temperature. Write another query get the 5 records which have the lowest temperature.

[sort operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/sortoperator)
[top operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/topoperator)


---
#### Task 6: Reorder, rename, add columns 🎓
Current temperature is in Fahrenheit.Write a query to convert Fahrenheit temperatures to Celsius temperatures. For readability, show the Fahrenheit temperature and the Celsius temperaturesa as the 2 left-most columns. You can use the following formula: 
C = (F – 32) * (5.0/9.0) <br>
Take 5 random records from the past week.
Hint 1: 'project' operator provides lot more features
Hint 2: We used 5.0 and 9.0, rather than 5 and 9 to ensure these numbers were to the 'real' data type (double-precision floating-point format), rather than 'long' (a signed integer, Int64)

Expected result:</br>
<img src="/assets/images/temp.png" width="600">

[extend operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/extendoperator)
[project-rename operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectrenameoperator)
[project-reorder operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/projectreorderoperator)

---
#### Task 7: Total number of records 🎓
Write a query to find out how many records are in the table. 

[count operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/countoperator)

---
#### Task 8: Aggregations and string operations 🎓
Write a query to find out how many records have deviceId starting with 'x'. <br>
Write another query to find out how many records have deviceId starting with 'x', per device ID (aggregated by deviceId).</br>
Expected result for the second query:</br>
<img src="/assets/images/count_by.png" width="250">

[String operators - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/datatypes-string-operators)

[summarize operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
#### Task 9: Render a chart 🎓
Write a query to find out how many records startswith "x" , per device ID (aggregated by device ID) and render a piechart.

Expected result:</br>
<img src="/assets/images/pie.png" width="500">

[render operator - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/renderoperator?pivots=azuredataexplorer)

---
#### Task 10: Create bins and visualize time series 🎓
Write a query to show a timechart of the number of records over time. Use 10 minute bins (buckets). Each point on the timechart represent the number of devices on that bucket.

Expected result:</br>
<img src="/assets/images/chart.png" width="650">

[bin() - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction)

---
#### Task 11: Aggregations with time series visualizations 🎓
Write a query to show a timechart of the **average temperature** over time. Use 30 minute bins (buckets) Each point on the timechart represent the average temperature in that 30 min period.
Hint: summarize avg(Temp) by bin(enqueuedTime, 30m) 

Expected result:</br>
<img src="/assets/images/timeseries.png" width="650">

[summarize operator](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator)

---
### Challenge 4: Explore and Transform Data

In this challenge we will explore 3 capabilities of Data Explorer

- **Update Policy** is like an internal ETL. It can help you manipulate or enrich the data as it gets ingested into the source table (e.g. extracting JSON into separate columns, creating a new calculated column, joining the new records with a static dimension table that is already in your database, etc). For these cases, using an update policy is a very common and powerful practice.
Each time records get ingested into the source table, the update policy's qeury (which we'll define in the update policy) will run on them (and only on newly ingested records - other existing records in the source table aren’t visible to the update policy when it runs), and the results of the query will be appended to the target table. This function output schema and target table schema should exactly match.

- **Materialized views** expose an aggregation query over a source table, or over another materialized view. Materialized views always return an up-to-date result of the aggregation query (always fresh). Querying a materialized view is more performant than running the aggregation directly over the source table.

- **User-defined functions** are reusable subqueries that can be defined as part of the query itself (ad-hoc functions), or persisted as part of the database metadata (stored functions). User-defined functions are invoked through a name, are provided with zero or more input arguments (which can be scalar or tabular), and produce a single value (which can be scalar or tabular) based on the function body.

Expected Learning Outcomes:
- Create user defined functions to use repeatable logic
- Create an update policy to transform the data at ingestion time
- Create Materialized view to create a performant way to query source table with an aggregate 

For the next task, we will use the LogisticsTelemetryHistorical table .

#### Task 1: User defined Functions (Stored Functions) 🎓

Create a stored function that will contain the code of the following logic. Make sure the function works.

- Creates a calculated column NumofTagsCalculated = TotalTags - LostTags
- Projects only 4 columns - deviceId, enqueuedTime, NumofTagsCalculated, Temp

See the [create function](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/functions) article.

---

#### Task 2: Create an update policy 🎓
By taking 10 records, we can see that the telemetry column has a JSON structure. In this task, we will use an 'update policy' to manipulate the raw data in the LogisticsTelemetryHistorical table (the source table) and transform the JSON data into separate columns, that will be ingested into a new table that we’ll create (“target table”).
We want to create a new table, with a calculated column (we will call it: NumOfTagsCalculated) that contains the following value: telemetry.TotalTags - telemetry.LostTags.

The schema of the new (destination) table would be:
```
  ( deviceId:string, enqueuedTime:datetime, NumOfTagsCalculated:int, Temp:real)
```
```
Example (note that the order of the keys may be different):

{
  "BatteryLife": 73,
  "Light": "70720.236143472212",
  "Tilt": "18.608539012789223",
  "Humidity": "60.178854625386215",
  "Shock": "-4.6141182265359628",
  "Pressure": "529.61165751122712",
  "ActiveTags": 165,
  "TransportationMode": "Ocean",
  "Status": "Online",
  "LostTags": 9,
  "Temp": "7.5054504554744765",
  "TotalTags": 185,
  "Location": {
      "alt": 1361.0469,
      "lon": -107.7473,
      "lat": 36.0845
  }
}
```

**Build the target table**
```
.create table LogisticsTelemetryManipulated  (deviceId:string, enqueuedTime:datetime, NumOfTagsCalculated:long, Temp:real) 
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
  .set-or-append LogisticsTelemetryHistorical <| LogisticsTelemetryHistorical | take 1000
```
- [Kusto Ingest from Query | Microsoft Docs](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/management/data-ingestion/ingest-from-query)


Make sure the data is transformed correctly in the destination table
```
    LogisticsTelemetryManipulated
   | take 10
```

**Relevant docs for this challenge:**
  - [Kusto update policy - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/updatepolicy)

---
---
#### Task 3: Create a materialized view 🎓

Instead of writing a query every time to retrieve the last known value for every device, create a materialized view containing the last known value for every device (the last record for each deviceId, based on the enqueuedTime column)

[Materialized views - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview) </br>
[.create materialized view - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/materialized-views/materialized-view-create) </br>
Use arg_max(). See examples of [arg_min() (aggregation function) - Azure Data Explorer | Microsoft Docs](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/arg-min-aggfunction)

-----
#### Task 4: Materialized views queries 🎓

There are 2 ways to query a materialized view: query the entire view or query the materialized part only. Try both of them.<br>

[Materialized views queries
](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/management/materialized-views/materialized-view-overview#materialized-views-queries)

---

🎉 Congrats! You've completed ADX in a Day Lab 1. Keep going with [**Lab 2: Advanced KQL, Policies and Visualization**](https://github.com/Azure/ADX-in-a-Day-Lab2)