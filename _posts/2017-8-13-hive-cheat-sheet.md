---
layout: post
sitemap: true
noindex: false
title: Hive Cheat Sheet
categories: hive cheatsheet csa175 csa
---

[Apache Hive™](https://hive.apache.org/) is an open source project run by volunteers at the Apache Software Foundation, that facilitates reading, writing, and managing large datasets residing in distributed storage using SQL syntax. Here we have a basic cheatsheet for hive command line tool… enjoy!

**Index**
 - [Hive Command Line Basics](#hive-command-line-basics)
   - [Connect to Hive](#connect-to-hive)
   - [Basic operations](#basic-operations)
   - [Creating an external table](#creating-an-external-table)
   - [Create table like and Alter Table](#create-table-like-and-alter-table)
   - [Temporary Tables](#temporary-tables)
   - [Loading Data from Files and Existing Tables](#loading-data-from-files-and-existing-tables)
   - [Complex Data Types and Table Generating Functions](#complex-data-types-and-table-generating-functions)
      - [Array Data Type](#array-data-type)
      - [Map Data Type](#map-data-type)
      - [Struct Data Type](#struct-data-type)
    - [Built-in Functions](#built-in-functions)
      - [Explode() Function](#explode-function)
      - [Posexplode() Function](#posexplode-function)
    - [Lateral Views](#lateral-views)
    - [Constraints in Subqueries and Views](#constraints-in-subqueries-and-views)
      - [Set Operations in Hive](#set-operations-in-hive)
      - [Subqueries in the WHERE Clause](#subqueries-in-the-where-clause)
    - [Views](#views)

Built on top of [Apache Hadoop™](http://hadoop.apache.org/), Hive provides the following features:
 - Tools to enable easy access to data via SQL, thus enabling data warehousing tasks such as extract/transform/load (ETL), reporting, and data analysis.
 - A mechanism to impose structure on a variety of data formats
 - Access to files stored either directly in Apache HDFS™ or in other data storage systems such as Apache HBase™ 
 - Query execution via [Apache Tez™](http://tez.apache.org/), [Apache Spark™](http://spark.apache.org/), or [MapReduce](http://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html)
 - Procedural language with HPL-SQL
 - Sub-second query retrieval via [Hive LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP), [Apache YARN](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/YARN.html) and [Apache Slider](https://slider.incubator.apache.org/).


**Hive** stores its data in HDFS: Data is stored as files - text files, binary files - partitioned across machines in the cluster, replicated for fault tolerance. Processing tasks parallelized across multiple machines - Hive runs all processes in the form of MapReduce jobs under the hood.
  - MapReduce: A parallel programming model. Defines the logic to process data on multiple machines - Batch processing operations on files in HDFS

Hive database schema cannot be enforced on these files. Schema-on-read - Hive tries to impose this schema when data is read:
 - Number of columns, column types, constraints specified at table creation
 - It may not succeed, may pad data with **_nulls_**


**Hive Metastore** expose the file-based storage of HDFS in
the form of tables. **Metastore** is the bridge between data stored in files and the tables exposed to users
  - Stores metadata for all the tables in Hive 
  - Maps the files and directories in Hive to tables
  - Holds table definitions and the schema for each table
  - Has information on converting files to table representations
  - Any database with a JDBC driver can be used as a metastore
      - Development environments use the built-in Derby database


**Data** is stored in directories under Hive’s warehouse directory (usually in `/user/hive/warehouse` - CDH) - defined on an property in **hive-site.xml**: 
> hive.metastore.warehouse.dir

```
[cloudera@quickstart ~]$ hdfs dfs -ls /user/hive
Found 1 items
drwxrwxrwx   - hive supergroup          0 2017-08-12 14:08 /user/hive/warehouse
[cloudera@quickstart ~]$ hdfs dfs -ls /user/hive/warehouse
Found 4 items
drwxrwxrwx   - cloudera supergroup          0 2017-08-12 14:08 /user/hive/warehouse/customers
drwxrwxrwx   - cloudera supergroup          0 2017-08-12 14:07 /user/hive/warehouse/sample_07
drwxrwxrwx   - cloudera supergroup          0 2017-08-12 14:08 /user/hive/warehouse/sample_08
drwxrwxrwx   - cloudera supergroup          0 2017-08-12 14:08 /user/hive/warehouse/web_logs
[cloudera@quickstart ~]$ 
```

> **Hive Tables**

**Managed**: Data managed by Hive and stored in the warehouse directory
 - Hive owns the files and directories 
 - These can be modified by other technologies
 - Deleting a managed table deletes both data and metadata

**External**: Data not fully managed by Hive and exists outside the warehouse directory
 - Share the underlying data across other technologies (Hadoop, Pig, HBase, etc)
 - Deleting an external table deletes only the metadata

`Metadata for both (managed and external) tables are stored on the metastore`

**Temporary Table** : _TODO: describe_

# Hive Command Line Basics

## Connect to Hive
Connect or run commands / scripts in Hive using `beeline`
```
beeline -n cloudera -p cloudera -u jdbc:hive2:// 

beeline -u jdbc:hive2:// -e "select * from customers"

beeline -u jdbc:hive2:// -f test.hql 

```

Show databases
```
show databases;

show databases like 'pl*'
```

## Basic operations
```sql
create database mysampledb;
use mysampledb;

create table customers (
id bigint,
name string,
address string
);

show tables;
describe customers;

insert into customers values (1111, "John", "WA");

insert into customers values 
(2222, "Emily", "WA"), (3333, "Rick", "WA"),
(4444, "Jane", "CA"), (5555, "Amit", "NJ"), 
(6666, "Nina", "NY");

select * from customers where address = "WA";

select name, address from customers where address = "CA";

select name, address from customers where address = "WA" and id > 2222;

select DISTINCT address from customers;

select name, address from customers order by address;

select count(*) from customers;

select address, count(*) from customers group by address;

select address, count(*) as customer_count from customers group by address;

select * from customers limit 1;

create table if not exists orders (
id bigint, 
product_id string, 
customer_id bigint, 
quantity int, 
amount double
);

insert into orders values 
(111111,"phone",1111,3,1200);

insert into orders values
(111112, "camera", 1111, 1, 5200),
(111113, "broom", 1111, 1, 10),
(111114,"broom",2222,2,20),
(111115,"t-shirt",4444,2,66);

select customers.id, name, product_id 
from customers join orders 
where customers.id = orders.customer_id; 
```

Check on HDKS
```
hadoop fs  -ls /user/hive/warehouse

hadoop fs -ls /user/hive/warehouse/mysampledb.db

hadoop fs -ls /user/hive/warehouse/mysampledb.db/customers

hadoop fs -cat /user/hive/warehouse/mysampledb.db/customers/000000_0

hadoop fs -cat /user/hive/warehouse/mysampledb.db/customers/000000_0_copy_1

hadoop fs -cat /user/hive/warehouse/mysampledb.db/customers/000000_0_copy_2

```


### External Tables

create `products.csv` like below
```
iphone7, iPhone 7, 950
camera_canon, Canon 570x, 1000
washingmachine_samsung, Samsung Swift, 400
tv_vu, Vu 56 Inch, 600
```

Save `products.csv` to hdfs
```
[cloudera@quickstart ~]$ sudo -u hdfs hadoop fs -mkdir /data
[cloudera@quickstart ~]$ sudo -u hdfs hadoop fs -chown cloudera /data
[cloudera@quickstart ~]$ hadoop fs -copyFromLocal products.csv /data/ 
[cloudera@quickstart ~]$ hdfs dfs -ls /data
Found 1 items
-rw-r--r--   1 cloudera supergroup        120 2017-08-20 15:00 /data/products.csv
[cloudera@quickstart ~]$ 
```

## Creating an external table
Take a look at the options:
 - `row format delimited fields terminated by ','`
 - `location '/data/';`

```
create external table if not exists products (
id string,
title string,
cost float
)
comment "Table to store product information sold in stores"
row format delimited fields terminated by ','
stored as textfile
location '/data/';

select * from products;
+-------------------------+-----------------+----------------+--+
|       products.id       | products.title  | products.cost  |
+-------------------------+-----------------+----------------+--+
| iphone7                 |  iPhone 7       | 950.0          |
| camera_canon            |  Canon 570x     | 1000.0         |
| washingmachine_samsung  |  Samsung Swift  | 400.0          |
| tv_vu                   |  Vu 56 Inch     | 600.0          |
+-------------------------+-----------------+----------------+--+
4 rows selected (0.424 seconds)

describe formatted products;
+----------------------------------------------------+--+
|           col_name            |                     data_type                      |                      comment                       |
+-------------------------------+----------------------------------------------------
| # Detailed Table Information  | NULL                                               | NULL      
| Location:                     | hdfs://quickstart.cloudera:8020/data               | NULL      
| Table Type:                   | EXTERNAL_TABLE                                     | NULL      
| Table Parameters:             | COLUMN_STATS_ACCURATE                              | false     
|                               | EXTERNAL                                           | TRUE      
|                               | comment                                            | Table to store product information sold in stores  |


drop table products;

hadoop fs -ls /data/
#(The data is still here even after deleting the external table)
```

## Create table like and Alter Table
```
create table if not exists fresh_products like products;

show tables;

describe fresh_products;

alter table fresh_products 
rename to freshproducts;

show tables;

alter table freshproducts add columns (
expiry_date date 
comment "Expiry date of fresh produce"
);

describe freshproducts;

# The command below alter the column title, but do not move the data
alter table products
change column id id string
after title;

```

## Temporary Tables

 - Store temporary data 
 - Tables of the same name can be created by different users
 - Do not support partitions and indexes
 - Can have the same name as a permanent table


```
create temporary table test_customers like customers;

show tables;

describe test_customers;

insert into test_customers values (9999,"Jill","MN");

select * from test_customers;

!q

```
... quiting will _delete_ the temporary table


## Loading Data from Files and Existing Tables

create a loca file `freshproducs.csv`
```
broccolli, Broccoli, 5
spinach, Spinach, 7
carrot, Local Carrots, 4
potato, Idaho Potatoes, 4
```

Load from a local path (not from HDFS)
- will be a managed table
```
load data local inpath 'freshproducts.csv'
into table freshproducts; 
```

Loads from HDFS
 - will move from the file `freshproducts.csv` from the   `/data/` directory to Hive wharehouse directory (on CDH `/user/hive/wharehouse`)
```
load data inpath '/data/freshproducts.csv'
into table freshproducts; 
```

Loads from HDFS (overwriting existing table data)
```
load data inpath '/data/freshproducts.csv'
overwrite into table freshproducts; 
```

Multi-table Insert and Deleting Data from Tables
 - get data from one table and insert into others

```
from products
insert overwrite table product_name
select id, title
insert into table product_cost
select id, cost;
```

Delete table data (truncate)
```
truncate table tablename
```

## Complex Data Types and Table Generating Functions

### Array Data Type

Create a table with array data types
```
create table mobilephones (
id string,
title string,
cost float,
colors array<string>,
screen_size array<float>
);
```

Insert data into a table with arrays
> This operation can causes CBO - Cost Based Optimizer error - it's enabled by default... the query will succeed even with this error.

```
insert into table mobilephones
select "redminote7", "Redmi Note 7", 300, 
array("white", "silver", "black"), array(float(4.5))
UNION ALL
select "motoGplus", "Moto G Plus", 200, 
array("black", "gold"), array(float(4.5), float(5.5));

select * from mobilephones;

select id, colors from mobilephones;

select id, colors[0] from mobilephones;
```

Insert data into a table with arrays from a file

nano `mobilephones.csv`...
```
samsungj7, Samsung J7, 250, red#blue#black, 5.5,camera:true#dualsim:false,24 hours#2MP
oneplusthree, One Plus Three, 450, gold#silver, 4.5#5.5,autofocus:true,12 hours
```

HQL
```
create table mobilephones (
id string,
title string,
cost float,
colors array<string>,
screen_size array<float>
)
row format delimited fields terminated by ','
collection items terminated by '#';

load data local inpath 'mobilephones.csv'
into table mobilephones;

select * from mobilephones;

select id, colors from mobilephones;

select id, colors[0] from mobilephones;
```


### Map Data Type
```
drop table mobilephones;

create table mobilephones (
id string,
title string,
cost float,
colors array<string>,
screen_size array<float>,
features map<string, boolean>
)
row format delimited fields terminated by ','
collection items terminated by '#'
map keys terminated by ':';

select id, features['camera'] as has_camera from mobilephones;
```

### Struct Data Type
```
drop table mobilephones;

create table mobilephones (
id string,
title string,
cost float,
colors array<string>,
screen_size array<float>,
features map<string, boolean>,
information struct<battery:string,camera:string>
)
row format delimited fields terminated by ','
collection items terminated by '#'
map keys terminated by ':';

load data local inpath 'mobilephones.csv'
into table mobilephones;

select id, features['camera'] as has_camera, information.battery from mobilephones;

select * from mobilephones;
+------------------+---------------------+--------------------+--------------------------+---------------------------+----------------------------------+----------------------------------------+--+
| mobilephones.id  | mobilephones.title  | mobilephones.cost  |   mobilephones.colors    | mobilephones.screen_size  |      mobilephones.features       |        mobilephones.information        |
+------------------+---------------------+--------------------+--------------------------+---------------------------+----------------------------------+----------------------------------------+--+
| samsungj7        |  Samsung J7         | 250.0              | [" red","blue","black"]  | [5.5]                     | {"camera":true,"dualsim":false}  | {"battery":"24 hours","camera":"2MP"}  |
| oneplusthree     |  One Plus Three     | 450.0              | [" gold","silver"]       | [4.5,5.5]                 | {"autofocus":true}               | {"battery":"12 hours","camera":null}   |

```

## Built-in Functions

 - UDF - User Defined Function: _works on a single row, outputs a single row => Ex.: trim(), concat(), length(), round(), floor()_
 - UDAF - User-defined Aggregate function: _work on multiple rows, outputs a single row => Ex.: count(), sum(), avg()_
 - UDTF - User-defined Table-generating function: _works on a single row, outputs multiple rows => Ex.: explode(), posexplode()_

### Explode() Function

select explode(colors) as variants from mobilephones;
select explode(features) as (feature, present) from mobilephones;
select explode(features) as (feature, present) from mobilephones where id = 'samsungj7';

```
0: jdbc:hive2://> describe mobilephones;
OK
+--------------+---------------------------------------+----------+--+
|   col_name   |               data_type               | comment  |
+--------------+---------------------------------------+----------+--+
| id           | string                                |          |
| title        | string                                |          |
| cost         | float                                 |          |
| colors       | array<string>                         |          |
| screen_size  | array<float>                          |          |
| features     | map<string,boolean>                   |          |
| information  | struct<battery:string,camera:string>  |          |
+--------------+---------------------------------------+----------+--+
0: jdbc:hive2://> 
0: jdbc:hive2://> 
0: jdbc:hive2://> select colors from mobilephones;
OK
+--------------------------+--+
|          colors          |
+--------------------------+--+
| [" red","blue","black"]  |
| [" gold","silver"]       |
+--------------------------+--+
2 rows selected (0.152 seconds)

0: jdbc:hive2://> 
0: jdbc:hive2://> 
0: jdbc:hive2://> select explode(colors) as variants from mobilephones;
OK
+-----------+--+
| variants  |
+-----------+--+
|  red      |
| blue      |
| black     |
|  gold     |
| silver    |
+-----------+--+
0: jdbc:hive2://>
0: jdbc:hive2://>  
0: jdbc:hive2://> select features from mobilephones;
OK
+----------------------------------+--+
|             features             |
+----------------------------------+--+
| {"camera":true,"dualsim":false}  |
| {"autofocus":true}               |
+----------------------------------+--+
2 rows selected (0.131 seconds)
0: jdbc:hive2://> 
0: jdbc:hive2://> 
0: jdbc:hive2://> select explode(features) as (feature, present) from mobilephones where id = 'samsungj7';
OK
+------------+----------+--+
|  feature   | present  |
+------------+----------+--+
| camera     | true     |
| dualsim    | false    |
| autofocus  | true     |
+------------+----------+--+
3 rows selected (12.127 seconds)
0: jdbc:hive2://> 
```


### Posexplode() Function

select posexplode(colors) as (index, variants) from mobilephones;

```
0: jdbc:hive2://> select posexplode(colors) as (index, variants) from mobilephones;
OK
+--------+-----------+--+
| index  | variants  |
+--------+-----------+--+
| 0      |  red      |
| 1      | blue      |
| 2      | black     |
| 0      |  gold     |
| 1      | silver    |
+--------+-----------+--+
5 rows selected (13.544 seconds)
0: jdbc:hive2://> 
0: jdbc:hive2://> 
```

## Lateral View
A lateral view is a virtual table formed by the exploded view, wich can be joined with the orinignal table to allow comples queries

select id, variants from mobilephones
lateral view explode(colors) colorsTable as variants;

select id, feature, present from mobilephones
lateral view explode(features) featuresTable as feature, present;

select id, trim(variants) from mobilephones
lateral view explode(colors) colorsTable as variants;

```
0: jdbc:hive2://> select id, variants
. . . . . . . . > from mobilephones
. . . . . . . . > lateral view explode(colors) colorsTable as variants;
OK
+---------------+-----------+--+
|      id       | variants  |
+---------------+-----------+--+
| samsungj7     |  red      |
| samsungj7     | blue      |
| samsungj7     | black     |
| oneplusthree  |  gold     |
| oneplusthree  | silver    |
+---------------+-----------+--+
0: jdbc:hive2://> 
0: jdbc:hive2://> 
0: jdbc:hive2://> select id, feature, present from mobilephones
. . . . . . . . > lateral view explode(features) featuresTable as feature, present;
0: jdbc:hive2://> 
0: jdbc:hive2://> 
OK
+---------------+------------+----------+--+
|      id       |  feature   | present  |
+---------------+------------+----------+--+
| samsungj7     | camera     | true     |
| samsungj7     | dualsim    | false    |
| oneplusthree  | autofocus  | true     |
+---------------+------------+----------+--+
3 rows selected (15.21 seconds)
0: jdbc:hive2://> 
0: jdbc:hive2://> 
```

**_Another test_**

Create an CSV file `nano ads.csv`:
```
home_page,nike#moto#redmi#samsung#lg
phone_category_page,moto#iphone#redmi#micromax
homeappliance_category_page,samsung#lg
electronics_category_age,samsung#dell
```

Create table and load data
```
create table ads (
  page_id string,
  ad_list array<string>
)
row format delimited fields terminated by ','
collection items terminated by '#';

load data local inpath 'ads.csv'
into table ads;

select * from ads;


+------------------------------+-----------------------------------------+--+
|         ads.page_id          |               ads.ad_list               |
+------------------------------+-----------------------------------------+--+
| home_page                    | ["nike","moto","redmi","samsung","lg"]  |
| phone_category_page          | ["moto","iphone","redmi","micromax"]    |
| homeappliance_category_page  | ["samsung","lg"]                        |
| electronics_category_age     | ["samsung","dell"]                      |
+------------------------------+-----------------------------------------+--+

```

**_Working with lateral views_**

select explode(ad_list) from ads;

select ad_id, count(*) from ads
lateral view explode(ad_list) adListTable as ad_id
group by ad_id 
order by ad_id DESC;

```
0: jdbc:hive2://> describe ads;
OK
+-----------+----------------+----------+--+
| col_name  |   data_type    | comment  |
+-----------+----------------+----------+--+
| page_id   | string         |          |
| ad_list   | array<string>  |          |
+-----------+----------------+----------+--+
0: jdbc:hive2://> 
0: jdbc:hive2://> 
0: jdbc:hive2://> select explode(ad_list) from ads;
OK
+-----------+--+
|    col    |
+-----------+--+
| nike      |
| moto      |
| redmi     |
| samsung   |
| lg        |
| moto      |
| iphone    |
| redmi     |
| micromax  |
| samsung   |
| lg        |
| samsung   |
| dell      |
+-----------+--+
13 rows selected (13.267 seconds)
0: jdbc:hive2://>
0: jdbc:hive2://>
0: jdbc:hive2://> select page_id , ad_id from ads
. . . . . . . . > lateral view explode(ad_list) adListTable as ad_id;
OK
+------------------------------+-----------+--+
|           page_id            |   ad_id   |
+------------------------------+-----------+--+
| home_page                    | nike      |
| home_page                    | moto      |
| home_page                    | redmi     |
| home_page                    | samsung   |
| home_page                    | lg        |
| phone_category_page          | moto      |
| phone_category_page          | iphone    |
| phone_category_page          | redmi     |
| phone_category_page          | micromax  |
| homeappliance_category_page  | samsung   |
| homeappliance_category_page  | lg        |
| electronics_category_age     | samsung   |
| electronics_category_age     | dell      |
+------------------------------+-----------+--+
13 rows selected (13.747 seconds)
0: jdbc:hive2://>
0: jdbc:hive2://>
0: jdbc:hive2://> select ad_id, count(*) from ads
. . . . . . . . > lateral view explode(ad_list) adListTable as ad_id
. . . . . . . . > group by ad_id order by ad_id DESC;

OK
+-----------+------+--+
|   ad_id   | _c1  |
+-----------+------+--+
| samsung   | 3    |
| redmi     | 2    |
| nike      | 1    |
| moto      | 2    |
| micromax  | 1    |
| lg        | 2    |
| iphone    | 1    |
| dell      | 1    |
+-----------+------+--+

```
**_Multiple Lateral Views in One Query_**

Create an CSV file `nano student_subjects.csv`:
```
John#Jane#Jill,math#physics#chemistry
Emily#Ethan,english#biology
```

Crete the table `student_subjects` and load data into it:
```
create table student_subjects
(
  names array<string>,
  subjects array<string>
)
row format delimited fields terminated by ','
collection items terminated by '#';


load data local inpath student_subjects.csv
into table student_subjects;

select * from student_subjects;
+-------------------------+---------------------------------+--+
| student_subjects.names  |    student_subjects.subjects    |
+-------------------------+---------------------------------+--+
| ["John","Jane","Jill"]  | ["math","physics","chemistry"]  |
| ["Emily","Ethan"]       | ["english","biology"]           |
+-------------------------+---------------------------------+--+
```

**_Playing with data_**

```
select name, subject from student_subjects
lateral view explode(names) nameTable as name
lateral view explode(subjects) subjectTable as subject;
OK
+--------+------------+--+
|  name  |  subject   |
+--------+------------+--+
| John   | math       |
| John   | physics    |
| John   | chemistry  |
| Jane   | math       |
| Jane   | physics    |
| Jane   | chemistry  |
| Jill   | math       |
| Jill   | physics    |
| Jill   | chemistry  |
| Emily  | english    |
| Emily  | biology    |
| Ethan  | english    |
| Ethan  | biology    |
+--------+------------+--+

```

**_Nested nested_**


```
create table example ( double_nested array<array<int>>);

insert into table example
select array(array(1,2,3), array(4,5,6), array(7,8,9));

select * from example;
OK
+----------------------------+--+
|   example.double_nested    |
+----------------------------+--+
| [[1,2,3],[4,5,6],[7,8,9]]  |
+----------------------------+--+

select * from example
lateral view explode(double_nested) nestedTable as single_nested
lateral view explode(single_nested) flatTable as num;

OK
+----------------------------+----------------------------+----------------+--+
|   example.double_nested    | nestedtable.single_nested  | flattable.num  |
+----------------------------+----------------------------+----------------+--+
| [[1,2,3],[4,5,6],[7,8,9]]  | [1,2,3]                    | 1              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [1,2,3]                    | 2              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [1,2,3]                    | 3              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [4,5,6]                    | 4              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [4,5,6]                    | 5              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [4,5,6]                    | 6              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [7,8,9]                    | 7              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [7,8,9]                    | 8              |
| [[1,2,3],[4,5,6],[7,8,9]]  | [7,8,9]                    | 9              |
+----------------------------+----------------------------+----------------+--+
9 rows selected (13.151 seconds)

select num from example
lateral view explode(double_nested) nestedTable as single_nested
lateral view explode(single_nested) flatTable as num;

OK
+------+--+
| num  |
+------+--+
| 1    |
| 2    |
| 3    |
| 4    |
| 5    |
| 6    |
| 7    |
| 8    |
| 9    |
+------+--+

```

## Constraints in Subqueries and Views

### Set Operations in Hive

 - UNION: Combine results from 3 queries with the same column type. 
   - Union DISTINCT: removes duplicates
   - Union ALL: preserves duplicates


```
0: jdbc:hive2://> select * from products;
OK
+-------------------------+-----------------+----------------+--+
|       products.id       | products.title  | products.cost  |
+-------------------------+-----------------+----------------+--+
| iphone7                 |  iPhone 7       | 950.0          |
| camera_canon            |  Canon 570x     | 1000.0         |
| washingmachine_samsung  |  Samsung Swift  | 400.0          |
| tv_vu                   |  Vu 56 Inch     | 600.0          |
+-------------------------+-----------------+----------------+--+
4 rows selected (0.09 seconds)
0: jdbc:hive2://> select * from freshproducts;
OK
+-------------------+----------------------+---------------------+----------------------------+--+
| freshproducts.id  | freshproducts.title  | freshproducts.cost  | freshproducts.expiry_date  |
+-------------------+----------------------+---------------------+----------------------------+--+
| broccolli         |  Broccoli            | 5.0                 | NULL                       |
| spinach           |  Sssspinach          | 7.0                 | NULL                       |
| carrot            |  Local Carrots       | 4.0                 | NULL                       |
| potato            |  Idaho Potatoes      | 4.0                 | NULL                       |
+-------------------+----------------------+---------------------+----------------------------+--+
4 rows selected (0.097 seconds)
0: jdbc:hive2://>
0: jdbc:hive2://> select id, title, cost from products
. . . . . . . . > union all
. . . . . . . . > select id, title, cost from freshproducts;


select distinct(t.product_id) from
(
select product_id from
customers join orders
where customers.id = orders.customer_id
) t;

+---------------+--+
| t.product_id  |
+---------------+--+
| broom         |
| camera        |
| phone         |
| t-shirt       |
+---------------+--+
```

### Subqueries in the WHERE Clause

**IN/NOT IN**

```
select name from customers
where id in (1111, 3333, 5555);
+-------+--+
| name  |
+-------+--+
| John  |
| Rick  |
| Amit  |
+-------+--+

select id from customers
where id not in (1111, 3333, 5555);
+-------+--+
|  id   |
+-------+--+
| 2222  |
| 4444  |
| 6666  |
+-------+--+

select name from customers as c
where c.id in 
(select customer_id from orders);

+--------+--+
|  name  |
+--------+--+
| John   |
| Emily  |
| Jane   |
+--------+--+

select id, title from products as p
where p.id not in 
(select product_id from orders);

+-------------------------+-----------------+--+
|           id            |      title      |
+-------------------------+-----------------+--+
| iphone7                 |  iPhone 7       |
| camera_canon            |  Canon 570x     |
| washingmachine_samsung  |  Samsung Swift  |
| tv_vu                   |  Vu 56 Inch     |
+-------------------------+-----------------+--+
```

**EXISTS/NOT EXISTS**

With the _exists/not exists_ clause, the subquery must have at least one correlated predicate...

The query: 
`select id, title from products where id not in (select product_id from orders);`
should throw an error...

The correct way is:
```
select id from customers
where exists
(
select customer_id from orders
where orders.customer_id = customers.id
);

+-------+--+
|  id   |
+-------+--+
| 1111  |
| 2222  |
| 4444  |
+-------+--+
```

**Create Table and Insert Data with a Subquery**

The `create table` should not have the column name/types in this case...

```
# Will not work
create table allproducts 
(id string, name string, cost float) 
as select id, title as name, cost from products;

# This will work
create table allproducts
as
select id, title as name, cost from products;

```

## Views
Views are virtual tables with a subset of data from a larger, more extensive table. Views are stored as query in Hive's metastore, is executed just when it's used and updated when data in the underlying table changes. A view can contain data from a single or multime table. Views are read only, insert/load/alter will fail...

```
create view product_purchases
as
select product_id, quantity
from orders;

show tables;
+--------------------+--+
|      tab_name      |
+--------------------+--+
| ...                |
| product_purchases  |
| ...                |
+--------------------+--+

alter view product_purchases
as
select product_id, quantity, amount
from orders;

describe formatted product_purchases;
OK
+-------------------------------+----------------------------------------------------+-----------------------+--+
|           col_name            |                     data_type                      |        comment        |
+-------------------------------+----------------------------------------------------+-----------------------+--+
| Table Type:                   | VIRTUAL_VIEW                                       | NULL                  |
| InputFormat:                  | org.apache.hadoop.mapred.TextInputFormat           | NULL                  |
| OutputFormat:                 | org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat | NULL                  |
| Compressed:                   | No                                                 | NULL                  |
| # View Information            | NULL                                               | NULL                  |
| View Original Text:           | select product_id, quantity                        | NULL                  |
| from orders                   | NULL                                               | NULL                  |
| View Expanded Text:           | select `orders`.`product_id`, `orders`.`quantity`  | NULL                  |
| from `dbname`.`orders`        | NULL                                               | NULL                  |
+-------------------------------+----------------------------------------------------+-----------------------+--+


create view customer_purchases
as
select customer_id, product_id, address
from customers join orders
where customers.id = orders.customer_id;

select distinct(customer_id) from customer_purchases;
+--------------+--+
| customer_id  |
+--------------+--+
| 1111         |
| 2222         |
| 4444         |
+--------------+--+
```

## Partitioning and Bucketing
Design tables so queries run only on subsets of the dataset

### Partitioning

 - Split data into smaller subsets, separating records into manageable parts based on a column value
 - Data Splits may not be of the same size per partition
 - Each of these units will be stored in a different directory
 - hadoop fs -ls `/user/hive/warehouse/db/orders`
   - `/state=CA`
     - /file-01
     - /file-02
     - /file-03
   - `/state=NY`
   - `/state=WA`
   - `/state=NJ`

Table without partition
```
create table orders_no_partition
(
  id string,
  customer_id string,
  product_id string,
  quantity int,
  amount double,
  zipcode char(5),
  state char(2)
)
COMMENT 'Table with no partition applied'
row format delimited fields terminated by ',';

describe formatted orders_no_partition;
OK
+-------------------------------+----------------------------------------------------+--------------------------
|           col_name            |                     data_type                      |             comment
+-------------------------------+----------------------------------------------------+--------------------------
| # Detailed Table Information  | NULL                                               | NULL
| Location:                     | hdfs://quickstart.cloudera:8020/user/hive/warehouse/dbname.db/orders_no_partit
| Table Type:                   | MANAGED_TABLE                                      | NULL
+-------------------------------+----------------------------------------------------+--------------------------

```

Table without partition
```
create table orders_w_partition
(
  id string,
  customer_id string,
  product_id string,
  quantity int,
  amount double,
  zipcode char(5)
)
COMMENT 'Table with partition applied by state'
partitioned by (state char(2));

show tables like 'orders_w*';
OK
+---------------------+--+
|      tab_name       |
+---------------------+--+
| orders_w_partition  |
+---------------------+--+

show partitions orders_w_partition;
OK
+------------+--+
| partition  |
+------------+--+
+------------+--+

insert into orders_w_partition
partition(state="CA")
values
("o1", "c1", "p1", 1, 1.11, "90111"),
("o2", "c2", "p2", 1, 2.22, "90222"),
("o3", "c3", "p3", 1, 3.33, "90333"),
("o4", "c4", "p4", 1, 4.44, "90444");

insert into orders_w_partition
partition(state="WA")
values
("o10", "c10", "p10", 2, 10.11, "91111"),
("o20", "c20", "p20", 2, 20.22, "91222"),
("o30", "c30", "p30", 2, 30.33, "91333"),
("o40", "c40", "p40", 2, 40.44, "91444");

insert into orders_w_partition
partition(state="CA")
values ("o5", "c5", "p5", 1, 5.55, "90555");

insert into orders_w_partition
partition(state="NJ")
values
("o100", "c100", "p100", 3, 100.11, "92111"),
("o200", "c200", "p200", 3, 200.22, "92222");

insert into orders_w_partition
partition(state="NY")
values
("o201", "c201", "p201", 4, 201.22, "92122");


show partitions orders_w_partition;
OK
+------------+--+
| partition  |
+------------+--+
| state=CA   |
| state=NJ   |
| state=NY   |
| state=WA   |
+------------+--+
```

Partition is a virtual column...
```
 select id, customer_id,amount, state from orders_w_partition;
OK
+-------+--------------+---------+--------+--+
|  id   | customer_id  | amount  | state  |
+-------+--------------+---------+--------+--+
| o1    | c1           | 1.11    | CA     |
| o2    | c2           | 2.22    | CA     |
| o3    | c3           | 3.33    | CA     |
| o4    | c4           | 4.44    | CA     |
| o5    | c5           | 5.55    | CA     |
| o100  | c100         | 100.11  | NJ     |
| o200  | c200         | 200.22  | NJ     |
| o201  | c201         | 201.22  | NY     |
| o10   | c10          | 10.11   | WA     |
| o20   | c20          | 20.22   | WA     |
| o30   | c30          | 30.33   | WA     |
| o40   | c40          | 40.44   | WA     |
+-------+--------------+---------+--------+--+
```


One directory per partition...
```
[cloudera@quickstart ~]$ hadoop fs -ls -R /user/hive/warehouse/dbname.db/orders_w_partition
drwxrwxrwx   - root supergroup          0 2017-08-29 18:51 /user/hive/warehouse/dbname.db/orders_w_partition/state=CA
-rwxrwxrwx   1 root supergroup         88 2017-08-29 18:43 /user/hive/warehouse/dbname.db/orders_w_partition/state=CA/000000_0
-rwxrwxrwx   1 root supergroup         22 2017-08-29 18:51 /user/hive/warehouse/dbname.db/orders_w_partition/state=CA/000000_0_copy_1
drwxrwxrwx   - root supergroup          0 2017-08-29 18:56 /user/hive/warehouse/dbname.db/orders_w_partition/state=NJ
-rwxrwxrwx   1 root supergroup         60 2017-08-29 18:56 /user/hive/warehouse/dbname.db/orders_w_partition/state=NJ/000000_0
drwxrwxrwx   - root supergroup          0 2017-08-29 18:57 /user/hive/warehouse/dbname.db/orders_w_partition/state=NY
-rwxrwxrwx   1 root supergroup         30 2017-08-29 18:57 /user/hive/warehouse/dbname.db/orders_w_partition/state=NY/000000_0
drwxrwxrwx   - root supergroup          0 2017-08-29 18:49 /user/hive/warehouse/dbname.db/orders_w_partition/state=WA
-rwxrwxrwx   1 root supergroup        104 2017-08-29 18:49 /user/hive/warehouse/dbname.db/orders_w_partition/state=WA/000000_0
[cloudera@quickstart ~]$
[cloudera@quickstart ~]$
[cloudera@quickstart ~]$ hadoop fs -cat /user/hive/warehouse/dbname.db/orders_w_partition/state=CA/000000_0_copy_1
o5c5p515.5590555
```

**Loading from Files into a Partitioned Table**

```
drop table orders_w_partition;

create table orders_w_partition
(
  id string,
  customer_id string,
  product_id string,
  quantity int,
  amount double,
  zipcode char(5)
)
COMMENT 'Table with partition applied by state, load data from fs'
partitioned by (state char(2))
row format delimited fields terminated by ',';

```

create `nano orders_CA.csv`: 
```
o1, c1, p1, 1, 1.11, 90111
o2, c2, p2, 1, 2.22, 90222
o3, c3, p3, 1, 3.33, 90333
o4, c4, p4, 1, 4.44, 90444
```

create `nano orders_CT.csv`: 
```
o10, c10, p10, 10, 10.11, 900111
o20, c20, p20, 10, 20.22, 900222
o30, c30, p30, 10, 30.33, 900333
o40, c40, p40, 10, 40.44, 900444
```

Load data into hive tables
```
load data local inpath 'orders_CA.csv'
into table orders_w_partition
partition (state="CA");

load data local inpath 'orders_CT.csv'
into table orders_w_partition
partition (state="CT");

show partitions orders_w_partition;
OK
+------------+--+
| partition  |
+------------+--+
| state=CA   |
| state=CT   |
+------------+--+

```

**Partitioning an External Table**

Load data into hdfs: **one directory per partition**
```
[cloudera@quickstart ~]$ hadoop fs -mkdir -p /data/CA
[cloudera@quickstart ~]$ hadoop fs -mkdir -p /data/CT
[cloudera@quickstart ~]$ hadoop fs -copyFromLocal orders_CA.csv /data/CA
[cloudera@quickstart ~]$ hadoop fs -copyFromLocal orders_CT.csv /data/CT
[cloudera@quickstart ~]$ hdfs dfs -ls -R /data
drwxr-xr-x   - cloudera supergroup          0 2017-08-29 19:39 /data/CA
-rw-r--r--   1 cloudera supergroup        108 2017-08-29 19:39 /data/CA/orders_CA.csv
drwxr-xr-x   - cloudera supergroup          0 2017-08-29 19:40 /data/CT
-rw-r--r--   1 cloudera supergroup        132 2017-08-29 19:40 /data/CT/orders_CT.csv
-rw-r--r--   1 cloudera supergroup        120 2017-08-20 15:00 /data/products.csv
```

Drop and Create an external table
```
drop table orders_w_partition;

create external table orders_w_partition
(
  id string,
  customer_id string,
  product_id string,
  quantity int,
  amount double,
  zipcode char(5)
)
COMMENT 'Table with partition applied by state, load data from hdfs'
partitioned by (state char(2))
row format delimited fields terminated by ','
location '/data/';


alter table orders_w_partition
add partition (state='CA')
location '/data/CA';
OK

show partitions orders_w_partition;
OK
+------------+--+
| partition  |
+------------+--+
| state=CA   |
+------------+--+

alter table orders_w_partition
add partition (state='CT')
location '/data/CT';

show partitions orders_w_partition;
OK
+------------+--+
| partition  |
+------------+--+
| state=CA   |
| state=CT   |
+------------+--+

select id, state, customer_id, product_id from orders_w_partition;
OK
+------+--------+--------------+-------------+--+
|  id  | state  | customer_id  | product_id  |
+------+--------+--------------+-------------+--+
| o1   | CA     |  c1          |  p1         |
| o2   | CA     |  c2          |  p2         |
| o3   | CA     |  c3          |  p3         |
| o4   | CA     |  c4          |  p4         |
| o10  | CT     |  c10         |  p10        |
| o20  | CT     |  c20         |  p20        |
| o30  | CT     |  c30         |  p30        |
| o40  | CT     |  c40         |  p40        |
+------+--------+--------------+-------------+--+
8 rows selected (0.148 seconds)

```

Changing the file location of an partition (use **set** before location)
```
alter table orders_w_partition
partition (state='CT')
set location '/data/Connecticut'
```


### Bucketing
 - Size of each split should be the same
 - Hash of a column value
 - Each bucket is a separate file
 - Makes sampling and joining data more efficient

----

## Join Optimizations 
Join operations are MapReduce jobs under the hood, by reducing the amount of data held in memory usually by structuring joins as a map-only operation, or by storing small tables in memory...


----

## Window Functions


----

A suite of functions which are syntactic sugar for complex queries, reducing the need for intermediate tables to store temporary data



"Partitioning and Bucketing", "Partitioning", "Bucketing"
"Join Optimizations ",
"Window Functions"

