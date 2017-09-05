---
layout: post
sitemap: true
noindex: false
title: Sqoop Cheat Sheet
categories: sqoop cheatsheet csa175 csa
---

Sqoop is a tool designed to transfer data between Hadoop and relational databases or mainframes. You can use Sqoop to import data from a relational database management system (RDBMS) such as MySQL or Oracle or a mainframe into the Hadoop Distributed File System (HDFS), transform the data in Hadoop MapReduce, and then export the data back into an RDBMS. Here we have a basic cheatsheet of sqoop command line tool… enjoy!

**_Some Use Cases_**: 
 - **ELT**
    - Extract data from RDBMS and load data to HADOOP, transform data in HADOOP, and return results to RDBMS.
 - **ETL**
    - Extract data from RDBMS data to HADOOP, transform data in HADOOP, and load DWH (data to wharehouse).
 - **Data Archival**
    - Move data from RDBMS (after it expires) to Hadoop, keeping RDBMS "clean and lean"
 - **Move Reports to Hadoop**
 - **etc**



# Importing Data To Hadoop From A Relational Database

## Command Line Basics: Importing Data Using Sqoop

**_SQOOP CLI Basics_**

sqoop import `--connect jdbc:mysql://mysql.example.com/db_name --username sqoop --password sqoop` --table cities
 - use `-P` instend of `--password`


Using an option File:

sqoop `--options-file /home/cloudera/sqoop_option.txt` --table TEST`

[cloudera@quickstart ~]$ cat sqoop_option.txt
```
#
# Options file for Sqoop import
#

# Specifies the tool being invoked
import

# Connect parameter and value
--connect
jdbc:mysql://mysql.example.com/db_name 

# Username parameter and value
--username
sqoop

#
# Remaining options should be specified in the command line.
#
```

## Importing Data With Column Filters, Row Filters, And Free Text Queries

## Parallel Imports

## Import Data Directory To HIVE Tables

## Incremental Data Import Overview

## Incremental Data Import And Using Sqoop Stored Jobs


