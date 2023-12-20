# OPAL 101 – Getting Started with OPAL

Follow this tutorial to understand using OPAL to shape your data into business-focused information.

## Scenario
As the owner of a chain of Grab and Go stores, you want business information from your Point of Sale (PoS) to track sales data. From your PoS logs, you can extract data about the following information: 

* When items are purchased (Timestamp)
* Employee Name
* Employee ID
* Activity Performed
* Store Number
* Store Name
* City
* State


You can shape this data using OPAL to answer these questions:

* Most activity at a store
* Employee who handled each transaction
* Register ID for the transaction

In this tutorial, you learn to shape and model your data using OPAL, the language used by Observe to perform database tasks.

### Ingesting Data into Observe

Before working with OPAL, you must ingest your data into Observe using a Datastream and an Ingest Token. Use the following steps to start your ingestion process:

1. Log into your Observe instance, and click **Datastreams** from the left menu.


```{image} images/create-datastream.png
	:align: center
	:width: 800px
	:alt: Creating a Datastream for ingesting data
```
*<p align="center">Figure 1 - Creating a Datastream for Ingesting Data</p>*

4\. Click **Create Datastream**. 

5\. Enter *IntroductionToOPAL* as the **Name**. 

6\. For the **Description**, enter a short description of the Datastream. (Optional)

```{image} images/add-datastream.png
	:align: center
	:width: 300px
	:alt: Add Datastream Name and Description
```
*<p align="center">Figure 2 - Add Datastream Name and Description</p>*

7\. Click **Create**. 

```{image} images/datastream-details.png
	:align: center
	:width: 600px
	:alt: Datastream Created
```
*<p align="center">Figure 3 - Datastream Created</p>*

#### Adding an Ingest Token to Your Datastream

To allow data into your Datastream, you must create a unique Ingest Token. Use the following steps to create an **Ingest Token**:
1. To add a new Ingest Token, click **Create token**.
2. Enter *IntroductionToOPALToken* as the **Name**.
3. For the **Description**, enter a short description of the **Ingest Token**. (Optional)

```{image} images/ingest-token.png
	:align: center
	:width: 300px
	:alt: Add Ingest Token Name and Description
```
*<p align="center">Figure 4 - Add Ingest Token Name and Description</p>*

4\. Click **Continue**.

5\. You receive a confirmation message confirming the creation of your token creation and the token value.

6\. Click **Copy to clipboard** to copy the token value and then save it in a secure location. 

```{note}
You can only view the token once in the confirmation message. If you don't copy and paste the token into a secure location, and you lose the token, you have to create a new one.
```
7\. Once you copy it into a secure location, select the confirmation checkbox, and then click **Continue**. 

```{image} images/success-token.png
	:align: center
	:width: 450px
	:alt: Successfully creating an ingest token
```
*<p align="center">Figure 5 - Successfully creating an Ingest Token</p>*

8\. After you click **Continue**, Observe displays **Setup instructions** for using the new Ingest Token. 

```{image} images/setup-instructions.png
	:align: center
	:width: 450px
	:alt: Setup instructions for using the Ingest Token
```
*<p align="center">Figure 6 - Setup instructions for using the Ingest Token</p>*

The URL, *https://{customerID}.collect.observeinc.com/*, displays your customer ID and URL for ingesting data.

HTTP requests must include your customer ID and Ingest Token information.

You can view the **Setup instructions** on your Datastream's **About** tab. 

### Using cURL to Ingest Data into the Tutorial Datastream

This tutorial uses three log files containing PoS data that you must send to your Observe instance. Download the following log files:
<ul>
<li><a href="../../../_static/PoSActivity1.log" download>PoSActivity1.log</a></li>
<li><a href="../../../_static/PoSActivity2.log" download>PoSActivity2.log</a></li>
<li><a href="../../../_static/PoSActivity3.log" download>PoSActivity3.log</a></li>
</ul>

Or

Download the files [here](https://github.com/observeinc/tutorials). 

Copy the files to the following directory:

* **MacOS** - `home/<yourusername>`
* **Windows** - `C:\Users\<yourusername`

You send this data to the `v1/http` endpoint using cURL. See [EndPoints](https://docs.observeinc.com/en/latest/content/data-ingestion/endpoints/http.html) in the *Observe User Guide* for more details. 

**Details about the URL**
* The path component of the URL encoded as a tag.
* Query parameters also encoded as tags. 

To accomplish this, add the path, `OPAL101`, and the query parameter, `version`. Keep this information in mind for use later in the tutorial. 

Copy and paste the following cURL to ingest the log data into the Datastream. Replace ${CustomerID} with your Customer ID, such as `123456789012`. 

```text
curl "https://${CustomerID}.collect.observeinc.com/v1/http/OPAL101?version=version1" \
-H "Authorization: Bearer {token_value}" \
-H "Content-type: text/plain" \
--data-binary @./PoSActivity1.log
```
```text
curl "https://${CustomerID}.collect.observeinc.com/v1/http/OPAL101?version=version2" \
-H "Authorization: Bearer {token_value}" \
-H "Content-type: text/plain" \
--data-binary @./PoSActivity2.log
```
```text
curl "https://${CustomerID}.collect.observeinc.com/v1/http/OPAL101?version=version3" \
-H "Authorization: Bearer {token_value}" \
-H "Content-type: text/plain" \
--data-binary @./PoSActivity3.log
```
Each cURL command returns `{"ok":true}` when successful.

After ingesting the data, you should see the data in the Datastream, *IntroductionToOPAL*.

```{image} images/datastream-ingest.png
	:align: center
	:width: 600px
	:alt: Successfully ingesting data into the datastream
```
*<p align="center">Figure 7 - Successfully ingesting data into the datastream</p>*
1. Click **Open dataset** to view the ingested data. 
2. Select a row to display details for that log event.

```{image} images/event-details.png
	:align: center
	:width: 800px
	:alt: Displaying details about a row of ingested data
```
*<p align="center">Figure 8 - Displaying details about a row of ingested data</p>*

## Learning to Shape Data with OPAL

### What is OPAL?

An [OPAL](https://docs.observeinc.com/en/latest/content/query-language-reference/ObserveProcessingAndAnalysisLanguage.html) pipeline consists of a sequence of statements where the output of one statement is the input for the next. This could be a single line with one statement or many lines of complex shaping and filtering.

A pipeline contains four types of elements:

* **Inputs** - Define the applicable datasets.
* **Verbs** - Define what processing to perform with those datasets.
* **Functions** - Define transforming individual values in the data.
* **Outputs** - Pass a dataset on to the next verb or the final result.

In this tutorial, you focus on a few key verbs and functions as you experiment with your uploaded data.


### Creating a Worksheet for Your Dataset

To create a **Worksheet** to start modeling your data, click **Create Worksheet** at the top of your **IntroductionToOPAL** Dataset.  A **Worksheet** contains similarities to most spreadsheet applications. It uses column headings, columns, and rows to logically group your data. 

```{image} images/opal-console.png
	:align: center
	:width: 600px
	:alt: Creating a Worksheet from a Dataset
```
*<p align="center">Figure 9 - Creating a Worksheet from a Dataset</p>*

To begin modeling using OPAL, open the **OPAL console** located just under the **Worksheet**. 

### Shaping Your Data Using OPAL

#### Extracting Columns using the `make_col` verb

First, let’s experiment with semi-structured data. Semi-structured data can consist of data such as emails, comma-separated values, JSON, etc. 

When you ingested the data into Observe, you added a custom path and query string parameter to the HTTP ingestion endpoint. You can find these in the **Extra** column:

```{image} images/extra-col.png
	:align: center
	:width: 600px
	:alt: Displaying the Extra column
```
*<p align="center">Figure 10 - Displaying the Extra column</p>*

When you double-click on any incoming row cell in the **Extra** column, the **Inspect** window displays the following:

```{image} images/data-cell-details.png
	:align: center
	:width: 300px
	:alt: Displaying a Data Cell in the Inspect window
```
*<p align="center">Figure 11 - Displaying a Data Cell in the Inspect window</p>*

The data in the **Extra** column shows that the data has a *JSON* object format. You can manipulate this data using the [`make_col`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/make_col.html) projection operation. 

Enter the following code in the OPAL console and then click **Run**:

```text
// Extract the path and version of the sent file from the EXTRA column 
make_col path:string(EXTRA.path) 
make_col version:string(EXTRA.version)
```

```{image} images/new_columns.png
	:align: center
	:width: 600px
	:alt: Adding new columns from the Extra column
```
*<p align="center">Figure 12 - Adding new columns from the Extra column</p>*

Let’s look at the components of the OPAL expression and understand each one:

```text
make_col path:string(EXTRA.path)
//^^^^^^                         Make Column verb 
//       ^^^^                    The name of the resulting column ("path")
//            ^^^^^^             Casting of the value in the expression to "string" data type
//                   ^^^^^^^^^^^ The fully qualified path to the desired value in "EXTRA" column ("EXTRA.path")
```
Note that the [`make_col`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/make_col.html) verb can evaluate multiple expressions at once and span multiple lines in the code editor. 

The following OPAL expression combines both expressions into a single operation but provides the same output as the previous expression:

```text
// Extract the path and version of the sent file from the EXTRA column
make_col path:string(EXTRA.path), 
         version:string(EXTRA.version)
```
#### Filtering the Dataset using the `filter` verb

Next, let’s look at filtering using the OPAL language in Observe.

In the previous section, you extracted two columns, `path` and `version`, contained in the `Extra` column and converted them to strings. Now, learn to use columns with explicit equality [`filters`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/filter.html). 


Copy the following code, and paste it into your OPAL console. Click **Run** at the top of the OPAL console.


```text
// Filter to just the rows sent from the file with specific version 
filter path = "/OPAL101" 
filter version = "version3"
```
Now you have filtered the incoming dataset to display just one of the ingested log files.

```{image} images/version3.png
	:align: center
	:width: 600px
	:alt: Filtering to one specific ingested log file data
```
*<p align="center">Figure 13 - Filtering to one specific ingested log file data</p>*

Let’s understand the components of the expression:

```text
filter path = "/OPAL101" 
//^^^^	 		Filter verb 
//  ^^^^ 			Column to compare ("path")
//	         ^     		Predicate ("=") 
//	           ^^^^^^^^     Value to compare
```
But you want to evaluate all of the data, so remove the `filter version = "version3"` line of OPAL and click **Run** to revert back to all input versions.

<img src="images/tick-icon-30.png" alt="Wait icon" width="50" height="42" style="vertical-align:bottom;"> **Wait! Let's make sure your OPAL console has the correct input so far.**

<details>
<summary open>Code Sample</summary>

```text
// Extract the path and version of the sent file from the EXTRA column 
make_col path:string(EXTRA.path)
make_col version:string(EXTRA.version)
// Filter to just the rows sent from the file with specific version 
filter path = "/OPAL101"
```
Remember that you removed the OPAL expression <i>version:string(EXTRA.version)</i> so that you can model all versions of the data.
</details>

#### Column Extraction and Formatting using the `make_col` verb and `trim` function

This section reviews how multiple functions can be combined together to extract and clean data. The contents of the ingested log files can be found in the **Fields** column, specifically in the text property. Double-click on any row in the **Fields** column to review the data in the **Inspect** window:

```{image} images/field-details-2.png
	:align: center
	:width: 800px
	:alt: Display Field details
```
*<p align="center">Figure 14 - Details of the Fields column</p>*

The value details reveal line breaks at the end of each line, `\r\n`:

```text
{
  "text": "2023-03-17 05:31:47,541 INFO Session: A123 Program: POS.TransactionComplete [sessionInfo: state=OPEN employeeId=11304 employeeName=Neil Armstrong securityRole=40 autoLockTime=900000 training=false registerID=001 storeNumber=XY123 storeName=[Amazing Teddy Bears] storeCity=Seattle storeState=WA storeCountry=US] [transactionInfo: associateId=11304 txType=sales lineItemCount=6 subtotal=7808 tax=764 total=8572] systemTime=1680217291429\r\n"
}
```
Enter the following OPAL expression and click **Run** at the top of the OPAL console:

```text
// Extract message column and trim extra carriage return/line feed
make_col message:trim(string(FIELDS.text), "\r\n")
```
This expression applies the [trim](https://docs.observeinc.com/en/latest/content/query-language-reference/function/trim.html) function in addition to the string value extracted from the column and removes carriage returns and line feed characters.

The new message column displays the following information without unnecessary characters.

```{image} images/no-carriage-returns.png
	:align: center
	:width: 600px
	:alt: Removing carriage returns and line feed characters
```
*<p align="center">Figure 15 - Removing carriage returns and line feed characters</p>*

Here’s the explanation of the `trim` expression:

```text
make_col message:trim(string(FIELDS.text), "\r\n")
//^^^^^^                                          Make Column verb
//       ^^^^^^^                                  The name of the resulting column ("path")
//               ^^^^                             Trim function to remove characters from enclosed values
//                                          ^^^^  Characters that Trim function should remove
//                    ^^^^^^                      Casting of the value in the expression to "string" data type
//                           ^^^^^^^^^^^          The fully qualified path to the desired value in "FIELDS" column ("FIELDS.text")
```
#### Extracting Columns using `extract_regex` and regular expressions

Next, create columns using regular expressions.

The message column appears to contain interesting business data about your stores. Using the [`extract_regex`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/extract_regex.html) verb, you can easily convert that unstructured string into structured data. 

Enter the following code in the OPAL console, and click **Run**.

```text
// Extract log timestamp, error level and program
extract_regex message, /(?P<logtimestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3})\s(?P<errorlevel>INFO|DEBUG|ERROR|WARN).*Program:\s(?P<program>\w*\.\w*)/, "i"
// Extract session details
extract_regex message, /sessionInfo:\s(?P<sessioninfo>.*)]/, "i"
// Extract session ID 
extract_regex message, /Session:\s(?P<sessionid>\w\d{3})/, "i"
// Extract system timestamp
extract_regex message, /systemTime=(?P<systemtime>\d*)/, "i"
```
The new `logtimestamp`, `errorlevel`, `program`, `sessioninfo`, `sessionid`, and `systemtime` columns now display in the **Worksheet**.

```{image} images/regex-1.png
	:align: center
	:width: 600px
	:alt: Extracting column data using extract_regex
```
*<p align="center">Figure 16 - Extracting column data using extract_regex</p>*

Description of the expression components

```text
extract_regex message, /(?P<logtimestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3})\s(?P<errorlevel>INFO|DEBUG|ERROR|WARN).*Program:\s(?P<program>\w*\.\w*)/, "i"
//^^^^^^^^^^^ Extract columns with regular expressions verb
//            ^^^^^^^   Column to be parsed
//                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Named capture group called "logtimestamp" capturing sequence of numbers like "2023-03-17 05:31:47,541"
//                                                                                 ^^ Space character
//                                                                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Named capture group called "errorlevel" capturing values from the list of "INFO", "DEBUG", "ERROR" and "WARN"
//                                                                                                                        ^^ Any character
//                                                                                                                          ^^^^^^^^^^ Exact match of "Program: "
//                                                                                                                                     ^^^^^^^^^^^^^^^^^^^^ Named capture group called "program" capturing two strings separated by period
//                                                                                                                                                            ^^^ modifiers, in this case specifying case-insensitive parsing
```
<img src="images/tick-icon-30.png" alt="Wait icon" width="50" height="42" style="vertical-align:bottom;"> **Wait! Let's make sure your OPAL console has the correct input so far.**

<details>
<summary open>Code Sample</summary>

```text
//Extract the path and version of the sent file from the EXTRA column 
make_col path:string(EXTRA.path) 
make_col version:string(EXTRA.version)
// Filter to just the rows sent from the file with specific version 
filter path = "/OPAL101"
// Extract message column and trim extra carriage return/line feed
make_col message:trim(string(FIELDS.text), "\r\n")
// Extract log timestamp, error level and program
extract_regex message, /(?P<logtimestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3})\s(?P<errorlevel>INFO|DEBUG|ERROR|WARN).*Program:\s(?P<program>\w*\.\w*)/, "i"
// Extract session details
extract_regex message, /sessionInfo:\s(?P<sessioninfo>.*)]/, "i"
// Extract session ID
extract_regex message, /Session:\s(?P<sessionid>\w\d{3})/, "i"
// Extract system timestamp
extract_regex message, /systemTime=(?P<systemtime>\d*)/, "i"
```
</details>

#### Converting a Column to a Timestamp using the `parse_timestamp` Function

Your next step converts parsed values from their default string representation to more appropriate types of values.

OPAL uses [`parse_isotime`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/parse_isotime.html) to parse the ISO 8601 timestamp format, but the example data doesn’t appear to be ISO 8601 timestamp format. However, you can easily parse the timestamp with the [`parse_timestamp`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/parse_timestamp.html) function.

Enter the following expression into the OPAL console, and click **Run**. 

```text
// Parse timestamp
make_col logtimestamp:parse_timestamp(logtimestamp, "YYYY-MM-DD HH24:MI:SS,FF3")
```
The `logtimestamp` column of the [string](https://docs.observeinc.com/en/latest/content/query-language-reference/function/string.html) type already exists, but now you have overwritten it using the new `logtimestamp` column of `timestamp` type.

```{image} images/logtimestamp.png
	:align: center
	:width: 600px
	:alt: Creating a new `logtimestamp` column
```
*<p align="center">Figure 17 - Creating a new logtimestamp column</p>*

Description of the expression components

```text
make_col logtimestamp:parse_timestamp(logtimestamp, "YYYY-MM-DD HH24:MI:SS,FF3")
//^^^^^^                                                                        Make Column verb
//       ^^^^^^^^^^^^                                                           The name of the resulting column (also "logtimestamp")
//                                    ^^^^^^^^^^^^                              The name of the source column ("logtimestamp")
//                    ^^^^^^^^^^^^^^^                                           parse_timestamp function to parse the timestamp
//                                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^ Format of the timestamp (using Snowflake's time format specifiers)
```

#### Converting Timestamp columns using the `from_milliseconds` function

OPAL provides full support for Unix timestamps with [`from_nanoseconds`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/from_nanoseconds.html), [`from_milliseconds`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/from_milliseconds.html), and [`from_seconds`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/from_seconds.html) functions. Let's explore these a little more.

Enter the following expression into the OPAL console, and click **Run**.

```text
// Parse system timestamp into time
make_col systemTime:from_milliseconds(int64(systemtime))
```
OPAL column names are case-sensitive. Therefore, the example produces a new column `systemTime`, distinct and different from `systemtime` even though they only differ by case:

```{image} images/system-time.png
	:align: center
	:width: 600px
	:alt: Creating systemTime and systemtime columns
```
*<p align="center">Figure 18 - Creating systemTime and systemtime columns</p>*

Description of the expression components

```text
make_col systemTime:from_milliseconds(int64(systemtime))
// ^^^^^                                                Make Column verb
//       ^^^^^^^^^^                                     The name of the resulting column ("systemTime" with capital "T")
//                                          ^^^^^^^^^^  The name of the source column ("systemtime" with lower "t")
//                  ^^^^^^^^^^^^^^^^^                   Convert time from unix Milliseconds function
//                                    ^^^^^^            Convert string to 64-bit integer
```

#### Extracting from Columns using the parse_kvs function

OPAL simplifies many common parsing challenges, including key/value pair parsing using the [`parse_kvs`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/parse_kvs.html) function.

Enter the following expression into the OPAL console, and click **Run**.

```text
// Parse session information which are key/value pairs
make_col sessioninfo_KVPairs:parse_kvs(sessioninfo)

// Create session information columns
make_col 
    employeeName:string(sessioninfo_KVPairs.employeeName),
    employeeId:int64(sessioninfo_KVPairs.employeeId),
    storeCountry:string(sessioninfo_KVPairs.storeCountry),
    storeState:string(sessioninfo_KVPairs.storeState),
    storeCity:string(sessioninfo_KVPairs.storeCity),
    storeNumber:string(sessioninfo_KVPairs.storeNumber),
    registerID:string(sessioninfo_KVPairs.registerID),
    autoLockTime:int64(sessioninfo_KVPairs.autoLockTime)
    transaction_total:int64(sessioninfo_KVPairs.total),
    storeName:string(sessioninfo_KVPairs.storeName)

// Mark the numeric employeeId column enumerable so it shows better in UI
set_col_enum employeeId:true
```
The [`parse_kvs`](https://docs.observeinc.com/en/latest/content/query-language-reference/function/parse_kvs.html) function splits incoming data from commonly used delimiters, and you can easily add your own delimiters, and produces a column with an object with key/value pair properties.

```{image} images/parse_kvs.png
	:align: center
	:width: 800px
	:alt: Extracting columns using the parse_kvs function
```
*<p align="center">Figure 19 - Extracting columns using the parse_kvs function</p>*

The OPAL expression creates additional columns using the now familiar [`make_col`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/make_col.html) verb. 

```{image} images/select_row.png
	:align: center
	:width: 800px
	:alt: Data details of selected row
```
*<p align="center">Figure 20 - Data details of a selected row</p>*

When you click on the header of the numeric columns, such as `autoLockTime`, the right panel displays a **Range Selector**:

```{image} images/autolocktime.png
	:align: center
	:width: 800px
	:alt: Range Selector for a Numerical data column
```
*<p align="center">Figure 21 - Range Selector for a Numerical data column</p>*

But although a column `employeeId` is numeric, you want to enumerate all the possible values instead of showing the numeric range. Use the [`set_col_enum`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/set_col_enum.html) verb to mark a column as `enumeration`, and the right panel displays the enumeration of all the values.

```{image} images/employeeID.png
	:align: center
	:width: 600px
	:alt: Display values for employeeID
```
*<p align="center">Figure 22 - Display available values for the employeeID column</p>*

<img src="images/tick-icon-30.png" alt="Wait icon" width="50" height="42" style="vertical-align:bottom;"> **Wait! Let's make sure your OPAL console has the correct input so far.**

<details>
<summary open>Code Sample</summary>

```text
// Extract the path and version of the sent file from the EXTRA column
make_col path:string(EXTRA.path) 
make_col version:string(EXTRA.version)
// Filter to just the rows sent from the file with specific version<br> 
filter path = "/OPAL101"
// Extract message column and trim extra carriage return/line feed
make_col message:trim(string(FIELDS.text), "\r\n")
// Extract log timestamp, error level and program<br>
extract_regex message, /(?P<logtimestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3})\s(?P<errorlevel>INFO|DEBUG|ERROR|WARN).*Program:\s(?P<program>\w*\.\w*)/, "i"
// Extract session details
extract_regex message, /sessionInfo:\s(?P<sessioninfo>.*)]/, "i"
// Extract session ID
extract_regex message, /Session:\s(?P<sessionid>\w\d{3})/, "i"
// Extract system timestamp
extract_regex message, /systemTime=(?P<systemtime>\d*)/, "i"
// Parse timestamp
make_col logtimestamp:parse_timestamp(logtimestamp, "YYYY-MM-DD HH24:MI:SS,FF3")
// Parse system timestamp into time
make_col systemTime:from_milliseconds(int64(systemtime))
// Parse session information which are key/value pairs
make_col sessioninfo_KVPairs:parse_kvs(sessioninfo)
// Create session information columns
make_col
    employeeName:string(sessioninfo_KVPairs.employeeName),
    employeeId:int64(sessioninfo_KVPairs.employeeId),
    storeCountry:string(sessioninfo_KVPairs.storeCountry),
    storeState:string(sessioninfo_KVPairs.storeState),
    storeCity:string(sessioninfo_KVPairs.storeCity),
    storeNumber:string(sessioninfo_KVPairs.storeNumber),
    registerID:string(sessioninfo_KVPairs.registerID),
    autoLockTime:int64(sessioninfo_KVPairs.autoLockTime),
    transaction_total:int64(sessioninfo_KVPairs.total),
    storeName:string(sessioninfo_KVPairs.storeName)
// Mark the numeric employeeId column enumerable so it shows better in UI
set_col_enum employeeId:true
```
</details>

#### Selecting the Columns to Display using the `pick_col` verb

You've created a lot of intermediate columns so far in this tutorial. Now you can use the OPAL [`pick_col`](https://docs.observeinc.com/en/latest/content/query-language-reference/verb/pick_col.html) verb to choose the columns you want to see as the final result and optionally rename them.

Enter the following expression into the OPAL console, and click **Run**.

```text
// Choose the columns, sometimes renaming them. Drop all the others
pick_col 
    BUNDLE_TIMESTAMP,
    systemtime,
    logtimestamp,
    "Error Level":errorlevel,
    "Activity Performed":program,
    sessionid, autoLockTime,
    storeCity, storeState, storeCountry,
    storeNumber, registerID,
    employeeName, employeeId,    
    message,
    transaction_total, storeName
```
This example selects just the columns you want and in the exact sequence you want. The expression uses the column binding expression to rename the `errorLevel` column to `Error Level` and the `program` column to `Activity Performed`.

```{image} images/pick_column.png
	:align: center
	:width: 600px
	:alt: Displaying selected renamed columns
```
*<p align="center">Figure 23 - Displaying selected renamed columns</p>*

### Displaying Visualizations for Sales Data

Next, you want to view the sales data for each store by employee to see which store has the top salespeople. 

Add the following OPAL to your OPAL console:

```text
filter @."Activity Performed" = "POS.TransactionComplete"
timechart options(empty_bins:true), A_transaction_total_sum:sum(transaction_total), group_by(storeName, employeeName)
```
You can now view your stores by sales transactions by clicking **Visualize** and selecting **Pie Chart** from the **Visualization > Type** list. 

```{image} images/sales_data.png
	:align: center
	:width: 600px
	:alt: Display sales by store
```
*<p align="center">Figure 24 - Displaying Sales Transactions by Store</p>*

To visualize the data as a Line Chart, select **Line Chart** and click **Apply**.

```{image} images/line-graph-dash.png
	:align: center
	:width: 600px
	:alt: Display sales by store with a Line Chart
```
*<p align="center">Figure 25 - Displaying Sales Transactions by Store Using a Line Chart</p>*

Move your cursor over a datapoint to display information about it.

To visualize the data as a Bar Chart, select **Bar Chart** and click **Apply**. 

```{image} images/bar-chart.png
	:align: center
	:width: 600px
	:alt: Display sales by store with a Bar Chart
```
*<p align="center">Figure 26 - Displaying Sales Transactions by Store Using a Bar Chart</p>*

To visualize the data as a Stacked Area, select **Stacked Area** and click **Apply**. 

```{image} images/stacked-area.png
	:align: center
	:width: 600px
	:alt: Display sales by store with a Stacked Area
```
*<p align="center">Figure 27 - Displaying Sales Transactions by Store Using a Stacked Area</p>*

To visualize the data as a Single Stat, select **Single Stat**, and then under **Settings**, select *A_transaction_total_sum*. Click **Apply**. 

```{image} images/single-stat.png
	:align: center
	:width: 600px
	:alt: Display sales by store as a Single Stat
```
*<p align="center">Figure 28 - Displaying Sales Transactions by Store Using a Single Stat by Employee Name</p>*

Use the Visualization type that best presents your data to your team. 

### Wrapping Up the Tutorial

To wrap up the tutorial, enter a name for your Worksheet, such as OPAL101 Tutorial, and click **Save**.

When you review the column `message`, you can see the PoS activity including `Login StartTime`, `sessionInfo`, `AddItem`, and `TransactionComplete`. This data helps you understand the number of transactions per day, who transacted the sale, and when the transaction finished. 

### Complete OPAL Script Used in the Tutorial

```{code}

// Extract the path and version of the sent file from the EXTRA column 
make_col path:string(EXTRA.path) 
make_col version:string(EXTRA.version)

// Filter to just the rows sent from the file with specific version 
filter path = "/OPAL101" 

// Extract message column and trim extra carriage return/line feed
make_col message:trim(string(FIELDS.text), "\r\n")

// Extract log timestamp, error level and program
extract_regex message, /(?P<logtimestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3})\s(?P<errorlevel>INFO|DEBUG|ERROR|WARN).*Program:\s(?P<program>\w*\.\w*)/, "i"
// Extract session details
extract_regex message, /sessionInfo:\s(?P<sessioninfo>.*)]/, "i"
// Extract session ID 
extract_regex message, /Session:\s(?P<sessionid>\w\d{3})/, "i"
// Extract system timestamp
extract_regex message, /systemTime=(?P<systemtime>\d*)/, "i"

// Parse timestamp
make_col logtimestamp:parse_timestamp(logtimestamp, "YYYY-MM-DD HH24:MI:SS,FF3")

// Parse system timestamp into time
make_col systemTime:from_milliseconds(int64(systemtime))

// Parse session information which are key/value pairs
make_col sessioninfo_KVPairs:parse_kvs(sessioninfo)

// Create session information columns
make_col 
    employeeName:string(sessioninfo_KVPairs.employeeName),
    employeeId:int64(sessioninfo_KVPairs.employeeId),
    storeCountry:string(sessioninfo_KVPairs.storeCountry),
    storeState:string(sessioninfo_KVPairs.storeState),
    storeCity:string(sessioninfo_KVPairs.storeCity),
    storeNumber:string(sessioninfo_KVPairs.storeNumber),
    registerID:string(sessioninfo_KVPairs.registerID),
    autoLockTime:int64(sessioninfo_KVPairs.autoLockTime),
    transaction_total:int64(sessioninfo_KVPairs.total),
    storeName:string(sessioninfo_KVPairs.storeName)

// Mark the numeric employeeId column enumerable so it shows better in UI
set_col_enum employeeId:true

// Choose the columns, sometimes renaming them. Drop all the others
pick_col 
    BUNDLE_TIMESTAMP,
    systemTime,
    logtimestamp,
    "Error Level":errorlevel,
    "Activity Performed":program,
    sessionid, autoLockTime,
    storeCity, storeState, storeCountry,
    storeNumber, registerID,
    employeeName, employeeId,    
    message,
    transaction_total, storeName

    
// added for dashboarding tutorial
filter @."Activity Performed" = "POS.TransactionComplete"
timechart options(empty_bins:true), A_transaction_total_sum:sum(transaction_total), group_by(storeName, employeeName)
```

#### Summary of Key Takeaways from the OPAL 101 Tutorial

In this tutorial, you learned how to perform the following tasks:

* Creating a Datastream to ingest data
* Creating an Ingest Token for the Datastream
* Using cURL to ingest your PoS log files into Observe
* Creating a Worksheet from your Dataset
* Using OPAL to shape your data





