<!--ts-->
- [KQL (Kusto Query Lanugage)](#kql-kusto-query-lanugage)
  - [Hot keys](#hot-keys)
  - [Add comment in KQL](#add-comment-in-kql)
  - [Search](#search)
    - [Search whole table across all columns](#search-whole-table-across-all-columns)
    - [Search whole table on specific column](#search-whole-table-on-specific-column)
    - [Search partial keyword in a specific column](#search-partial-keyword-in-a-specific-column)
    - [Search word with wild card](#search-word-with-wild-card)
    - [Search word which appears in the end of the string](#search-word-which-appears-in-the-end-of-the-string)
  - [Logical operators](#logical-operators)
  - [Where](#where)
  - [Take](#take)
  - [Count](#count)
  - [Summarize](#summarize)
  - [dcount](#dcount)
  - [Extend](#extend)
  - [Project](#project)
  - [Distinct](#distinct)
  - [Scalars Operators](#scalars-operators)
    - [print](#print)
    - [now](#now)
    - [ago](#ago)
    - [sort](#sort)
    - [extract](#extract)
    - [parse](#parse)
    - [datetime](#datetime)
    - [Timespan Arithmetic](#timespan-arithmetic)
    - [Converting a timespan into an a duration measured in seconds](#converting-a-timespan-into-an-a-duration-measured-in-seconds)
    - [startof ...](#startof-)
      - [Start of year](#start-of-year)
      - [Start of hour](#start-of-hour)
    - [endof ...](#endof-)
      - [end of year](#end-of-year)
      - [end of hour](#end-of-hour)
    - [between](#between)
      - [Example #1](#example-1)
      - [Example #2](#example-2)
    - [format\_datetime](#format_datetime)
    - [format\_timespan](#format_timespan)
    - [if Condition](#if-condition)
    - [Top](#top)
    - [String operators](#string-operators)
      - [strcat](#strcat)
      - [Contains](#contains)
      - [countof](#countof)
    - [mv-expand](#mv-expand)
    - [bag\_pack](#bag_pack)
    - [Pack array](#pack-array)
  - [Advanced Aggregation](#advanced-aggregation)
    - [arg\_max and arg\_min](#arg_max-and-arg_min)
    - [Max](#max)
    - [Sum](#sum)
    - [Sumif](#sumif)
    - [Countif](#countif)
    - [Pick random row from table](#pick-random-row-from-table)
    - [Pick random value from specific column](#pick-random-value-from-specific-column)
    - [Percentiles](#percentiles)
  - [Working with Datasets](#working-with-datasets)
    - [let](#let)
    - [join](#join)
  - [Full outer-join flavor](#full-outer-join-flavor)
  - [Left anti-join flavor](#left-anti-join-flavor)
  - [Right semi-join flavor](#right-semi-join-flavor)
      - [Example 1:](#example-1-1)
      - [Example 2:](#example-2-1)
    - [union](#union)
      - [Example #1:](#example-1-2)
      - [Example #2:](#example-2-2)
    - [datatable](#datatable)
    - [getschema (Type of each column)](#getschema-type-of-each-column)
    - [Prev/Next](#prevnext)
      - [Example1](#example1)
      - [Example2](#example2)
    - [toscalar](#toscalar)
    - [row\_cumsum](#row_cumsum)
    - [materialize](#materialize)
  - [Time Series](#time-series)
    - [The range Command](#the-range-command)
      - [Example](#example)
    - [The make-series Command](#the-make-series-command)
      - [Example1](#example1-1)
      - [Example2](#example2-1)
      - [Example3](#example3)
    - [series\_fit\_line](#series_fit_line)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: gil_diy, at: Tue 01 Nov 2022 10:34:52 IST -->

<!--te-->

# KQL (Kusto Query Lanugage)

## Hot keys

Hot Key | Description|
---|---|
`Ctrl+Space` | Show all columns and functions


## Add comment in KQL

```
# This is a comment
```

To comment out or uncomment mulltiple lines, mark them all and press `Ctrl + /`

## Search

The search command

### Search whole table across all columns

```KQL
Perf | search "Memory"
```

The table name is **Perf**

This symbol **|**, is called **pipe**, is a technique for passind information from one program to another program.

in our example, we are piping the content of table Perf into -the search command.

search for word "Memory" in table named **Perf**

same as:

```KQL
Perf | search "memory"
```

Therfore we can easily see, the search is not case sensitive.

### Search whole table on specific column

```KQL
Perf | search CounterName=="Available MBytes"
```

### Search partial keyword in a specific column

```KQL
Perf | search CounterName:"MBytes"
```

Use colon for **partial keyword search**

### Search word with wild card 

```KQL
Perf | search "*Bytes*"
```

`*` - means any character which may apear multiple times 

### Search word which appears in the end of the string

```KQL
Perf | search * endswith "Bytes"
```

Looking in the table **Perf** over all columns the word **Bytes** in the end of the string. 


```KQL
Perf | search * startswith "130"
```
In case, we would like to search for a prefix, we will use the **startswith** a function.

```KQL
Perf | search "Free*bytes"  and ("c:" or "d:")
```

Here we have table perf which we are searching in it,
we are looking for  "Free*bytes" and another constraint
is the rows should hold either "c:" or "d:" 

```KQL
Perf | search InstanceName matches regex "[A-D]:"
```

we are looking for in a specific column **InstanceName**, 
We are filtering the rows with regex constraint:
The constraint is all Letters between A to D and colon after.

## Logical operators

| Meaning      | Symbol | Example | result|
| ----------- | ----------- |----| -----|
| Not      | `!` or `not`     | `print not (5>3)` | `false`
| And   | `and`       | `print (5>3) and (6<7)` | `true`
| Or    | `or` | `print  ((5>3) or (3>10))` | `true`



## Where
```
Perf
| where TimeGenerated >= ago(1h) 
|  where (CounterName == "Bytes Received/sec"
        or
        CounterName == "% Processor Time"
        )
| where CounterValue > 0
```
Here we have 3 pipes after each pipe there is constraiht,
usind the **where** command we are focusing on specific columns.

The exact command can be written like this:


```
Perf
| where TimeGenerated >= ago(1h) 
   and (CounterName == "Bytes Received/sec"
        or
        CounterName == "% Processor Time"
        )
   and CounterValue > 0
```


## Take
```
Perf | take 7 
```
Here we are retrieving 7 rows randomly.
Next time, we will run the same command we will get different rows.

* The command **take** is the same as **limit**

## Count
```
Perf | count
```
Here we are counting the number of rows in table perf.

```
Perf
| where TimeGenerated >= ago(1h) 
   and (CounterName == "Bytes Received/sec"
   and CounterValue > 0
| count
```

here we are performing filtering on table perf and afterwards we check the number of rows in the end result.

## Summarize

```
Perf | summarize count() by ObjectName
```
<p align="center">
  <img src="images\summerize_example.jpg" width="1000">
</p>

Combining the summarize function with count function outputs the count of each distinct value in the column ObjectName.



**Example #2**

```
Perf | summarize column_counter=count() by ObjectName, CounterName
```

Combining the summarize function with count function outputs the count of each distinct value in the columns both ObjectName and CounterName. finally we rename the **count_** column to **column_counter**


**Example #3**

Here we are creating histogram by two columns (Computer, KubeEventType) and afterwards we sort by Computer and  KubeEventType by ascending order

```
KubeEvents
| project Computer, KubeEventType
| summarize count() by Computer, KubeEventType
| sort by Computer asc, KubeEventType asc
```


**Example #4**

```
Perf | summarize NumberOfEntries=count()
               by bin(TimeGenerated,1d)
```

Here we are focusing on Perf table, We want to count the number of rows in each day (According to column TimeGenerated), therefore, we use the function bin to generate actuall bins. (each day represents a single bin)


**Example #4**

```
Perf | where CounterName =="% Free Space" 
| summarize NumberOfRowsAtThisPrecentLevel=count()
         by bin(CounterValue,10)
```

Here we are focusing on table **Perf**, now we apply a filter
on all rows with constraint the value of CounterName should be "% Free Space", afterwards we split the rows into buckets (bins), each bucket of size 10. Eventually, we get a histogram where all bins appear.


**Example #5**

```
let Source = datatable(Name:string, Version:string)
[
    'Car', '1.0.0',
    'Train', '2.0.0',
    'Train', '1.0.0',
    'Car', '2.0.0'
];

Source| summarize make_set(Name) by Version
```



## dcount

Usually dcount is used with summarized 
(for estimating the cardinality of huge sets) 
It trades accuracy for performance
```
SecurityEvent
| where TimeGenerated >= ago(90d)
| summarize dcount(EventID) by Computer
| sort by Computer asc
```

## Extend


**Example 1**
```
Perf | where  CounterName == "Free Megabytes"
   | extend FreeGB=CounterValue/1000

```
Here we are focusing on table **Perf**, now we apply a filter
on all rows with constraint the value of CounterName should be "Free Megabytes", afterwards we would like to add another column therefore will use the extend function.
The new column name with be FreeGB.

**Example 2**
```
Perf| where TimeGenerated >=ago(10m)
     | extend ObjectCounter = strcat(ObjectName,"-",CounterName) 
```
Here we are focusing on table **Perf**, now we apply a filter
on all rows with constraint the value of TimeGenerated should be larger than 10 minutes. afterwards, we would like to add a new column which will hold the concatenation of each value in 2 columns (ObjectName, CounterName).

## Project

**Example 1**
```
Perf | project ObjectName, SourceSystem
```
Here we are picking 2 columns from table **Perf**.



**Example 2**

```
Perf | where CounterName == "Free Megabytes"
     |  project ObjectName,
                CounterName,
                InstanceName,
                TimeGenerated,
                FreeGB= CounterValue/1000,
                FreeMB= CounterValue,
                FreeKB= CounterValue*1000
                
```                
Here we are focusing on table **Perf**, now we apply a filter
on all rows with constraint the value of CounterName should be "Free Megabytes", afterwards we pick 4 columns (ObjectName,
                CounterName,
                InstanceName,
                TimeGenerated),
                FreeMB= CounterValue,
and add 3 columns (FreeGB, FreeMB, FreeKB).

**Example 3**

The **opposite** of the function **project** is **project-away**,
it will discard the columns we have mentioned in the query.
In the example below we are discarding the following columns:
TenantId,SourceSystem , CounterPath , CounterValue.

```
Perf | where TimeGenerated > ago(1h)
     |  project-away TenantId,SourceSystem , CounterPath , MG, CounterValue
```

## Distinct

The command distinct returns a list of values which are not repetitive.

Example: ['Mory', 'Gil', 'Omer', 'Omer', 'Mory'] -> ['Mory', 'Gil', 'Omer'] 

```
Perf | distinct InstanceName
```

## Scalars Operators

[scalar-data-types - very informative](https://github.com/microsoft/Kusto-Query-Language/tree/master/doc/scalar-data-types)


### print

print command is used for printing messages on the screen

```
print "Mory!!"
```
evaluate mathemtical expressions:

```
print 11*18
```

### now

returns the current datetime

```
print now()
```

### ago

brings us the datetime according to the value we have passed.

Gives us yesterday at the hour and minute, which means **exactly 24 hours before**:

```
print ago(1d)
```

```
print ago(1h)
```

```
print ago(1m)
```

```
print ago(1s)
```

```
print ago(1ms)
```

Gives us tomorrow:

```
print ago(-1d)
```



### sort

Sort table alphabetically the rows according to a specific column.

```
Perf | sort by Computer
```

You can use the keyword:

* `asc` - Ascending order

* `desc` - Descending order

### extract



Using the extract command we can retrieve the relevant characters 
which satisfy the constraints. 

**Example #1**

In our case we requested below all characters,
which are alphabetical (numbers will get discarded).

```
print extract("([a-z]+)", 0, "454846mory455")
```

**Example #2**

```
Perf | where ObjectName = "LogicalDisk" and Instancename matches regex "[A-Z]:"
| project Computer, CounterName, extract("([A-Z]):", 1, InstanceName)
```


### parse

The parse command is very helpful for splitting a string 
into multiple columns.
you should pipe the string and then parse it with columns names.
In our examples the columns' names are: 
* *age_column* 
* *weight_column*
* *name_column*

so all values after the keywords:
* "Age:"
* ",weight:"
* ",name:"

are set into the corresponding columns.


```
print my_value = 'Age:15,weight:35Kg ,name:Roni'
| parse my_value with  "Age:" age_column 
                    ",weight:" weight_column
                    ",name:" name_column
```


<p align="center">
  <img src="images\parse_example.jpg" width="1000">
</p>


### datetime

Here we are adding a new column which holds the amount of time past since Time-generated to now.


**Example #1:**

```
Perf | extend time_diff = now() - TimeGenerated
```

**Example #2:**

```
Perf |
extend hour=datetime_part("Hour", TimeGenerated) |
extend isAfternoon = hour > 7 |
summarize count() by isAfternoon
```


### Timespan Arithmetic

### Converting a timespan into an a duration measured in seconds

```
input_table
| extend Duration = (Timestamp_1 - Timestamp_2) /1s
```

### startof ...

#### Start of year

```
Perf | extend beginning_of_year = startofyear(TimeGenerated)
```

#### Start of hour

````
Perf | extend beginning_of_hour = startofhour(TimeGenerated)
````

### endof ...

#### end of year

```
Perf | extend end_of_year = endofyear(TimeGenerated)
```

#### end of hour

```
Perf | extend end_of_hour = endofhour(TimeGenerated)
```

### between

We can use between while using where,
for retrieving rows which staisfy the between constraint.

#### Example #1

```
Perf 
| where CounterName == "% Free Space" 
| where CounterValue between (70.0..100.0)
```

#### Example #2
```
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
  | where PreciseTimeStamp between(min_peak_t..max_peak_t)
```

### format_datetime

```
Perf 
| take 100
| project CounterName,
          CounterValue,
          TimeGenerated,
          format_datetime(TimeGenerated,"y-M-d"),
          format_datetime(TimeGenerated,"yyyy-MM-dd")
```

<p align="center">
  <img src="images\format_date_time_example.jpg" width="1000">
</p>

### format_timespan

### if Condition

We can easily use an if condition, 
in the example below we check the value `CounterValue`,
if it's below 50 then the value would be: **"You might want to look at this"** otherwise the value would be: **You're OK!**

```
Perf 
| where CounterName == "% Free Space"
| extend FreeState = iif(CounterValue <50, 
                        "You might want to look at this",
                        "You're OK!")
| project Computer , CounterName, CounterValue, FreeState 
```

### Top

In Perf table, we would like to sort in desc order by `CounterValue` and get the top 20 rows.
which means the 20 largest value of `CounterValue`.

```
Perf | top 20 by CounterValue
```

### String operators

#### strcat

```
strcat("My age is: ", my_age)
```

#### Contains

```
dogs 
| where Common_Health_Problems contains "eye problems" or  Common_Health_Problems contains "eye issues" 
```

#### countof

The **number of times that the search value can be matched** in the source string

```
dogs | extend number_of_problems = countof(Common_Health_Problems,',')+1;
```

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/countoffunction

### mv-expand

Expands multi-value dynamic arrays or property bags into multiple records (rows).

For example,
A simple expansion of a single column:

```
datatable (a:int, b:dynamic)
[1,dynamic({"prop1":"a1", "prop2":"b1"}),
 2,dynamic({"prop1":"a2", "prop2":"b2"})]
| mv-expand b
```

<p align="center">
  <img src="images\mv_expand.jpg" width="1500">
</p>

### bag_pack

given a list of values, convert to dictionary (multiple keys and values)

```KQL
print bag_pack("Level", "Information", "ProcessID", 1234, "Omri",29)
```

<p align="center">
  <img src="images\bag_pack_output.jpg" width="400">
</p>



### Pack array

```KQL
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
```

<p align="center">
  <img src="images\simple_table.jpg" width="200">
</p>

```KQL
range x from 1 to 3 step 1
| extend y = x * 2
| extend z = y * 2
| project s1 = pack_array(x,y,z), s2 = pack_array(z, y, x)
```

<p align="center">
  <img src="images\pack_array.jpg" width="200">
</p>


## Advanced Aggregation

### arg_max and arg_min

**Example #1:**
In case we would like to retrieve the whole row, which holds the minimum CounterValue. we will write:

```
Perf | summarize  arg_min(CounterValue,*)
```

**Reminder:** For getting the whole row of either minimum or maximum value we use `arg_min` or `arg_max`.

**Example #2:**

Here we are focusing on Perf table,
we would like to group all rows with the same `CounterName`, therefore we have used:
summarize ____ by CounterName
from each group we would like to get the row with max **CounterValue** , after we got all maximums of all groups we sort the rows by CounterName.



```
Perf 
| summarize arg_max(CounterValue, *) by CounterName
| sort by CounterName asc
```

### Max

Here after applying constraint on column `CounterName`, we would like to get the row which consists the maximum value of
`CounterValue` .

```
Perf 
| where CounterName == "Free Megabytes" 
| summarize max(CounterValue)
```

### Sum


Here after applying constraint on column `CounterName`, we would like to get the sum of column:
`CounterValue` 

```
Perf 
| where CounterName == "Free Megabytes" 
| summarize sum(CounterValue)
```

### Sumif


Here we calculate the sum of cells only if the same row satisfies the constriant: `CounterName == "Free Megabytes" `
```
Perf
| summarize sumif(CounterValue, CounterName == "Free Megabytes" )
```

### Countif

We would like to create an histogram only for the CounterName which contains "Bytes".

```
Perf
| summarize RowCount= countif(CounterName  contains "Bytes") by CounterName
| sort by CounterName asc 
```

### Pick random row from table

```
Perf 
| summarize any(*)
```

### Pick random value from specific column

Here we would like to pick a random cell from the column
`Computer`.

```
Perf | summarize  any(Computer)
```

Takes a random value from both columns:

```
Perf | summarize  any(Computer,CounterName)
```


Here we return a random row for each distinct value
in the clumn adter the by clause

```
Perf 
| summarize any(*) by CounterName
| sort by CounterName asc
```

### Percentiles

Run this example,
here we are renaming the generated columns 
(`percentile_CounterValue_5`, `percentile_CounterValue_30`, `percentile_CounterValue_95` )

```
Perf
| where CounterName == "Available MBytes"
| summarize percentiles(CounterValue, 5,30,95) by Computer
| project-rename Percent05 = percentile_CounterValue_5
                 ,Percent30 = percentile_CounterValue_30
                 ,Percent95 = percentile_CounterValue_95
```

## Working with Datasets

### let

**Important**: to run the code in `Microsoft Azure` select all the code and then press `Run`.   

let means to store a value in a variable

```
let minCounterValue = 300;
let countername ="Free Megabytes";

Perf 
| project Computer,
         TimeGenerated,
         CounterName,
         CounterValue
| where CounterName  == countername and CounterValue <= minCounterValue
```

* We can easily write a function using a `let` statement,
here we write a function named: `dateDiffInDays` 
The functions recieves two arguments (`date1`, `date2`), and then in the body of the function we calculate the difference between those dates in days.

```
let dateDiffInDays = (date1: datetime , date2: datetime =datetime(2018-01-01))
                     { (date1-date2)/1d}
                     ;

print dateDiffInDays(now(), todatetime("2018-05-01"))
```

* We can easily write a function using a let statement, here we write a function named: get_earlier_date The functions recieves two arguments (date1, date2), and then in the body of the function we find which date is earlier and return the date

```
let get_earlier_date = (date1: datetime , date2: datetime)
                     {iif(date1>date2,date2,date1)}
                     ;

print get_earlier_date(todatetime("2017-05-01"), todatetime("2002-01-01"))
```

### join

([Reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer#join-flavors))[]

## Full outer-join flavor

A full outer-join combines the effect of applying both left and right outer-joins. Whenever records in the joined tables don't match, the result set will have null values for every column of the table that lacks a matching row. For those records that do match, a single row will be produced in the result set, containing fields populated from both tables.

```

let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=fullouter Y on Key

```

## Left anti-join flavor

Left anti-join returns all records from the left side that don't match any record from the right side.

```

let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=leftanti Y on Key

```
## Right semi-join flavor

Right semi-join returns all records from the right side that match a record from the left side. Only columns from the right side are returned.

```
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X | join kind=rightsemi Y on Key
```

#### Example 1:

Here we are joining the two tables (`Perf`, `VMComputer`) which have common column named `Computer`, 

```
Perf 
| where  TimeGenerated >= ago(30d)
| take 1000
| join (VMComputer) on Computer
```

#### Example 2:

```
let startTime= ago(1d);
let endTime = now();
let procData= (
 Perf 
 | where TimeGenerated between (startTime..endTime)
 | where CounterName == "% Processor Time"
 | where ObjectName == "Processor"
 | where InstanceName =="_Total"
 | summarize PctCpuTime = avg(CounterValue)
    by Computer, bin(TimeGenerated, 1h)
 );

 let MemData=(
 Perf
  | where TimeGenerated between (startTime..endTime)
  | where CounterName == "Available MBytes"
  | summarize AvailableMB = avg(CounterValue)
    by Computer, bin(TimeGenerated, 1h)
);

procData | join kind= inner (MemData) 
on Computer, TimeGenerated
| project TimeGenerated, Computer, PctCpuTime , AvailableMB
| sort by TimeGenerated desc, Computer asc
```


### union

#### Example #1:

Here we conduct union between two tables:
`UpdateSummary`, `Update`. the union will output all rows one table below the other. For understanding which row came from which table we have added the `withsource="SourceTable"`
```
UpdateSummary 
| union withsource="SourceTable" Update
```

#### Example #2:

Attention the default value of the union operation is `inner`.

**Union outer**

**Returns all columns from both tables:**

```
union kind=outer withsource="SourceTable"
UpdateSummary,
Update
```

### datatable

Here we are creating a sample table, with 4 columns and the data inside it.

```
datatable (ID:int , TimeGenerated:datetime , YouTubeName:string, YouTubeURL:string)
[
1,datetime(2018-04-01),'Rocket','www.Rocket.com',
2,datetime(2018-04-01),'Dog','www.Dog.com',
3,datetime(2018-04-01),'Cat','www.Cat.com'
]
```


### getschema (Type of each column)

Show for each column in a specific table the
columns types

```
Perf | getschema 
```

### Prev/Next

For using the prev or next function we must use the `serialize` function.

#### Example1

```
let SomeData = datatable (rowNum:int , rowVal:string )
[
1,"Value 01",
2,"Value 02",
3,"Value 03",
4,"Value 04",
5,"Value 05",
];

SomeData 
| serialize 
| extend prVal = strcat("Previous Value was ", prev(rowVal))
| extend nextVal = strcat("Next Value was ", next(rowVal))
```


#### Example2

Here we are calculating the moving average on `PctCpuTime` for three consecutive rows.

```
let startTime = ago(1d);
let endTime = now();

Perf
| where TimeGenerated between (startTime .. endTime)
| where Computer == "Contosoweb"
| where CounterName == "% Processor Time"
| where ObjectName == "Processor"
| where InstanceName == " Total"
| summarize PctCpuTime = avg(CounterValue) by bin(TimeGenerated, 1h)
| sort by TimeGenerated asc
| extend movAvg = (PctCpuTime + prev(PctCpuTime,1,0)+ prev(PctCpuTime,2,0))/3.0
```


### toscalar

Returns a **scalar constant value** of the evaluated expression.

This function is useful for queries that require staged calculations. For example, calculate a total count of events, and then use the result to filter groups that exceed a certain percent of all events.

```
let avg_Longevity= (dogs | summarize avg(Longevity_min));

dogs 
| where (Longevity_min < toscalar(avg_Longevity)) | count;
```

### row_cumsum

For calculating the Cumulative sum we must use the function `serialize`.

```
datatable  (a:long)
[1,2,3,4,5,6,7,8,9,10] | serialize cumulativesum= row_cumsum(a)
```

### materialize

Captures the value of a tabular expression for the duration of the query execution so that it can be **referenced multiple times by the query without recalculation**.

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/materializefunction

## Time Series

### The range Command

Here, we'll creat a table for a range, one for each then, for the start of the day. then we'll join that a query getting the number of events bucketed by date. We'll then add a column that nicely formats the date. It then sorts the results and displays them.

#### Example

```
range  LastWeek from ago(7d) to now() step 1d
| project TimeGenerated= startofday(LastWeek)
| join  kind=fullouter (Event
                        |where  TimeGenerated > ago(7d)
                        | summarize Count= count()
                        by bin(TimeGenerated,1d)
) on  TimeGenerated 
| extend TimeDisplay= format_datetime(TimeGenerated,"yyyy-mm-dd")
|sort by TimeGenerated desc 
|project  TimeDisplay, Count  
          
```

### The make-series Command

We can use make series to generate an array of averages and times, then use mvexpand to pivot them back to rows.


#### Example1

```KQL
let data=datatable(timestamp:datetime, metric: real)
[
  datetime(2017-01-01), 4,
  datetime(2017-01-01), 8,
  datetime(2017-01-02), 15,
  datetime(2017-01-04), 7,
  datetime(2017-01-04T13:40), 13,
];
let interval = 1d;
let stime = datetime(2017-01-01);
let etime = datetime(2017-01-5);
data
| make-series avg(metric) on timestamp from stime to etime step interval 
```
We got a series of 4 elements,
each element holds the average of the metric in those corresponding days.

<p align="center">
  <img src="images\make_series.jpg" width="1500">
</p>

#### Example2

```
let startTime=ago(2h);
let endTime=now();
Perf
| where TimeGenerated between (startTime..endTime)
| where CounterName == " % Processor Time"
    and  Computer == " ContosoWeb1.ContosoRetail.com"
    and ObjectName == "process"
    and InstanceName !in ("_Total", "Idle")
| make-series avg(CounterValue) default=0
           on TimeGenerated in range (startTime,endTime,10m)   
           by InstanceName
| mv-expand TimeGenerated to typeof(datetime)
           , avg_CounterValue to typeof(double)
             limit 100000 
| render  timechart  
```


#### Example3

```
StorageBlobLogs
| where TimeGenerated > ago(1d) and OperationName has "PutBlob" and StatusText contains "success"
| summarize dcount(Uri) by bin(TimeGenerated, 1h)
| render timechart
```

<p align="center">
  <img src="images\render_time_chart.jpg" width="1500">
</p>



### series_fit_line

Finding a linear line for given dots (x,y)

Slope (https://en.wikipedia.org/wiki/Slope)

```
range x from 1 to 1 step 1
| project x=range(bin(now(),1h)-11h, bin(now(), 1h) ,1h)
        , y = dynamic([2,5,6,8,11,15,17,18,25,26,30,30])
| extend (RSquare, Slope, Variance, Rvariance, Interception, LineFit) = series_fit_line(y)
```

(url)[https://github.com/ManagedSentinel/AzureSentinelKQLScripts/blob/master/SeriesFitLineEstimateUsage.kql]