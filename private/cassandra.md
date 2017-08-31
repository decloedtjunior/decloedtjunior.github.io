---
layout: page
sitemap: false
noindex: true
title: Cassandra CheatSheet
permalink: /cassandra/
---

Apache Cassandra is an open source, distributed, decentralized, elastically scalable, highly available, fault-tolerant, tuneably consistent, row-oriented database that bases its _distribution design_ on Amazon’s Dynamo and its _data model_ on Google’s Bigtable.  Created at Facebook, it is now used at some of the most popular sites on the Web.

> scaling data stores means making certain trade-offs between data consistency, node availability, and partition tolerance.

When considering consistency, availability, and partition tolerance, we can achieve only two of these goals in a given distributed system, a trade-off known as the CAP theorem

In Cassandra, consistency is not an all-or-nothing proposition. We might more accurately term it “tuneable consistency” because the client can control the number of replicas to block on for all updates. This is done by setting the consistency level against the replication factor. The replication factor lets you decide how much you want to pay in performance to gain more consistency. The consistency level is a setting that clients must specify on every operation and that allows you to decide how many replicas in the cluster must acknowledge a write operation or respond to a read operation in order to be considered successful. 

**Row-Oriented**

Cassandra’s data model can be described as a partitioned row store, in which data is stored in sparse multidimensional hashtables. “Sparse” means that for any given row you can have one or more columns, but each row doesn’t need to have all the same columns as other rows like it (as in a relational model). “Partitioned” means that each row has a unique key which makes its data accessible, and the keys are used to distribute the rows across multiple data stores. 

In the relational storage model, all of the columns for a table are defined beforehand and space is allocated for each column whether it is populated or not. In contrast, Cassandra stores data in a multidimensional, sorted hash table. As data is stored in each column, it is stored as a separate entry in the hash table. Column values are stored according to a consistent sort order, omitting columns that are not populated, which enables more efficient storage and query processing.


Is Cassandra “Schema-Free”?
> In its early versions. Cassandra was faithful to the original Bigtable whitepaper in supporting a “schema-free” data model in which new columns can be defined dynamically.  Schema-free databases such as Bigtable and MongoDB have the advantage of being very extensible and highly performant in accessing large amounts of data. The major drawback of schema-free databases is the difficulty in determining the meaning and format of data, which limits the ability to perform complex queries. These disadvantages proved a barrier to adoption for many, especially as startup projects which benefitted from the initial flexibility matured into more complex enterprises involving multiple developers and administrators.The solution for those users was the introduction of the Cassandra Query Language (CQL), which provides a way to define schema via a syntax similar to the Structured Query Language (SQL) familiar to those coming from a relational background. Initially, CQL was provided as another interface to Cassandra alongside the schema-free interface based on the Apache Thrift project. During this transitional phase, the term “Schema-optional” was used to describe that data models could be defined by schema using CQL, but could also be dynamically extended to add new columns via the Thrift API. During this period, the underlying data storage continued to be based on the Bigtable model.Starting with the 3.0 release, the Thrift-based API that supported dynamic column creation has been deprecated, and Cassandra’s underlying storage has been re-implemented to more closely align with CQL. Cassandra does not entirely limit the ability to dynamically extend the schema on the fly, but the way it works is significantly different. CQL collections such as lists, sets, and especially maps provide the ability to add content in a less structured form that can be leveraged to extend an existing schema. CQL also provides the ability to change the type of columns in certain instances, and facilities to support the storage of JSON-formatted text.So perhaps the best way to describe Cassandra’s current posture is that it supports “flexible schema.”


Cassandra uses a special primary key called a composite key (or compound key) to represent wide rows, also called partitions. The composite key consists of a partition key, plus an  optional set of clustering columns.  The partition key is used to determine the nodes on which rows are stored and can itself consist of multiple columns. The clustering columns are used to control how data is sorted for storage within a partition.  Cassandra also supports an additional construct called a static column, which is for storing data that is not part of the primary key but is shared by every row in a partition.

![_config.yml]({{ site.baseurl }}/images/cassandra_wide_row.png)


Putting this all together, we have the basic Cassandra data structures:	
- The _column_, which is a name/value pair
- The _row_, which is a container for columns referenced by a primary key	
- The _table_, which is a container for rows
- The _keyspace_, which is a container for tables	
- The _cluster_, which is a container for keyspaces that spans one or more nodes



# Cassandra Cluster on Docker

> Start the first node
```
docker run --name=n1 -d tobert/cassandra
```

> Get the IP of the node
```
docker inspect -f "{{ .NetworkSettings.IPAddress}}" n1
```

> Change the **seeds** ip's with your docker, to start other two nodes
```
docker run --name=n2 -d tobert/cassandra -seeds 172.17.0.7
docker run --name=n3 -d tobert/cassandra -seeds 172.17.0.7
```

> Checking the cluster `docker exec -it n1 bash`

```
# nodetool status
UN -> Up, Normal
UJ -> Up, Joining
```

> Check the distribuition of node tokens
```
nodetool ring
```

The default number is defined on cassandra configuration file
```
cat /data/conf/cassandra.yaml
```

----

> Multi-Datacenter
 - -dc _datacenter_
 - -rack _rack_

**Token ranges are allocated by datacenter**

```
docker run --name=n1 -d tobert/cassandra -dc DC1 -rack RAC1 -seeds x.x.x.x
docker run --name=n2 -d tobert/cassandra -dc DC1 -rack RAC2 -seeds x.x.x.x
docker run --name=n2 -d tobert/cassandra -dc DC2 -rack RAC1 -seeds x.x.x.x
docker exec -it n1 cqlsh
```

```
create keyspace mytestkeyspace with replication = { 'class': 'SimpleStrategy', 'replication_factor':3};
create keyspace mytestkeyspace with replication = { 'class': 'NetworkTopologyStrategy', 'DC1':2, 'DC2':1};
```

```
nodetool describering mytestkeyspace
nodetool status mytestkeyspace
```

```
cqlsh> DESCRIBE CLUSTER;

Cluster: Docker Cluster
Partitioner: Murmur3Partitioner

cqlsh>
cqlsh> DESCRIBE KEYSPACES;

system_traces  mytestkeyspace  system

cqlsh> SHOW VERSION;
[cqlsh 5.0.1 | Cassandra 2.1.7 | CQL spec 3.2.0 | Native protocol v3]
cqlsh>
cqlsh>
cqlsh> DESCRIBE KEYSPACE my_keyspace

CREATE KEYSPACE my_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'}  AND durable_writes = true;

cqlsh>
cqlsh> USE my_keyspace ;
cqlsh:my_keyspace>
cqlsh:my_keyspace> CREATE TABLE user 
( first_name text ,   last_name text, PRIMARY KEY (first_name)) ;
cqlsh:my_keyspace> DESCRIBE TABLE user;

CREATE TABLE my_keyspace.user (
    first_name text PRIMARY KEY,
    last_name text
) WITH bloom_filter_fp_chance = 0.01
    AND caching = '{"keys":"ALL", "rows_per_partition":"NONE"}'
    AND comment = ''
    AND compaction = {'min_threshold': '4', 'class': 'org.apache.cassandra.db.compaction.SizeTieredCompactionStrategy', 'max_threshold': '32'}
    AND compression = {'sstable_compression': 'org.apache.cassandra.io.compress.LZ4Compressor'}
    AND dclocal_read_repair_chance = 0.1
    AND default_time_to_live = 0
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND read_repair_chance = 0.0
    AND speculative_retry = '99.0PERCENTILE';

cqlsh:my_keyspace>
cqlsh:my_keyspace> INSERT INTO user (first_name, last_name )   VALUES ('Bill', 'Nguyen');
cqlsh:my_keyspace> SELECT COUNT (*) FROM user;

 count
-------
     1

(1 rows)
cqlsh:my_keyspace> SELECT * FROM user WHERE first_name='Bill';

 first_name | last_name
------------+-----------
       Bill |    Nguyen

(1 rows)
cqlsh:my_keyspace> DELETE last_name FROM USER WHERE   first_name='Bill';
cqlsh:my_keyspace> SELECT * FROM user WHERE first_name='Bill';

 first_name | last_name
------------+-----------
       Bill |      null

(1 rows)
cqlsh:my_keyspace>
TRUNCATE user;
cqlsh:my_keyspace> SELECT * FROM user;

 first_name | last_name
------------+-----------

(0 rows)
cqlsh:my_keyspace> DROP TABLE user;
cqlsh:my_keyspace>


```



# The Cassandra Query Language

```
USE my_keyspace ;

CREATE TABLE user 
( first_name text ,   last_name text, PRIMARY KEY (first_name)) ;

SELECT * FROM user WHERE first_name='Bill';
```

Alter table
```
ALTER TABLE user ADD title text;

DESCRIBE TABLE user;

INSERT INTO user (first_name, last_name, title)   VALUES ('Bill', 'Nguyen', 'Mr.');

INSERT INTO user (first_name, last_name) VALUES   ('Mary', 'Rodriguez');

SELECT * FROM user;

```

view the timestamps that were generated for our previous writes by adding the _writetime()_ function
```
SELECT first_name, last_name,   writetime(last_name) FROM user;

/*
should throw an error, since we are not allowed to ask for the timestamp on **primary key** columns
*/
 SELECT WRITETIME(first_name) FROM user;
```

_USING TIMESTAMP_ option to manually set a timestamp (note that the timestamp must be later than the one from our SELECT command, or the UPDATE will be ignored)
```
UPDATE user USING TIMESTAMP 1434373756626000   SET last_name = 'Boateng' WHERE first_name = 'Mary' ;

SELECT first_name, last_name,   WRITETIME(last_name) FROM user WHERE first_name = 'Mary';

```
Setting the timestamp is not required for writes. This functionality is typically used for writes in which there is a concern that some of the writes may cause fresh data to be overwritten with stale data. This is advanced behavior and should be used with caution.

Time to live (TTL)
Cassandra provides is the ability to expire data that is no longer needed... it works at the level of individual column values. The TTL value defaults to null.

```
SELECT first_name, last_name, TTL(last_name)   FROM user WHERE first_name = 'Mary';

//Set TTL
UPDATE user USING TTL 3600 SET last_name =   'McDonald' WHERE first_name = 'Mary' ;

SELECT first_name, last_name, TTL(last_name)   FROM user WHERE first_name = 'Mary';   

```
TTL is stored on a per-column level. There is currently no mechanism for setting TTL at a row level directly. As with the timestamp, there is no way to obtain or set the TTL value of a primary key column, and the TTL can only be set for a column when we provide a value for the column.

# CQL Types

## Numeric Data Types

- int (32-bit as in Java), bigint (64-bit equivalent to a Java long), smallint (16-bit), tinyint	(8-bit), varint (variable precision, equivalent to java.math.BigInteger)
- float	(A 32-bit IEEE-754 floating point as in Java),	double	(A 64-bit IEEE-754 floating point as in Java),	decimal	(equivalent to java.math.BigDecimal)

## Textual Data Types
- text, varchar : UTF-8 character string
- ascii	: ASCII character string

## Time and Identity Data Types
 - timestamp (each column has a timestamp indicating when it was last modified)
 - date, time
 - uuid: Type 4 UUID (Cassandra defaults), based entirely on random numbers.
 - timeuuid: Type 1 UUID, which is based on the MAC address of the computer, the system time, and a sequence number used to prevent duplicates. 


```
ALTER TABLE user ADD id uuid;

UPDATE user SET id = uuid() WHERE first_name =   'Mary';

SELECT first_name, id FROM user WHERE   first_name = 'Mary';

```

Notice that the id is in UUID format.
```
 first_name | id
------------+--------------------------------------
       Mary | c256e4ff-d68d-42cf-8c97-9322a55ea60e
```

## Other Simple Data Types
 - boolean, blob, inet, counter (only incremented or decremented)

## Collections

 - **set** stores a collection of unordered elements, but cqlsh returns the elements in sorted order. One advantage of using set is the ability to insert additional items without having to read the contents first.

```
ALTER TABLE user ADD emails set<text>;
UPDATE user SET emails = {   'mary@example.com' } WHERE first_name = 'Mary';
SELECT emails FROM user WHERE first_name =   'Mary';

 emails
----------------------
 {'mary@example.com'}

(1 rows)

UPDATE user SET emails = emails + { 'mary.mcdonald.AZ@gmail.com' } 
WHERE first_name = 'Mary';
SELECT emails FROM user WHERE first_name =   'Mary';

 emails
----------------------------------------------------
 {'mary.mcdonald.AZ@gmail.com', 'mary@example.com'}

(1 rows)
```
Other Set Operations	
We can also clear items from the set by using the subtraction operator: 
> SET emails = emails - {'mary@example.com'}

Alternatively, we could clear out the entire set by using the empty set notation: 

> SET emails = {}


- **list** contains an ordered list of elements. By default, the values are stored in order of insertion.

```
ALTER TABLE user ADD   phone_numbers list<text>;

UPDATE user SET phone_numbers = [   '1-800-999-9999' ] WHERE first_name = 'Mary';

SELECT phone_numbers FROM user WHERE   first_name = 'Mary';

UPDATE user SET phone_numbers =   phone_numbers + [ '480-111-1111' ] 
WHERE first_name = 'Mary';

SELECT phone_numbers FROM user WHERE   first_name = 'Mary';

// Update by index
UPDATE user SET phone_numbers[1] =   '480-111-1111' WHERE first_name = 'Mary';

//Remove an item with subtraction
UPDATE user SET phone_numbers =   phone_numbers - [ '480-111-1111' ] 
WHERE first_name = 'Mary';

//Delete by index
DELETE phone_numbers[0] from user WHERE   first_name = 'Mary';

```

- **map** contains a collection of key/value pairs. The keys and the values can be of any type except counter.

```
ALTER TABLE user ADD   login_sessions map<timeuuid, int>;

UPDATE user SET login_sessions =  
{ now(): 13, now(): 18} WHERE first_name = 'Mary';

SELECT login_sessions FROM user WHERE   first_name = 'Mary';


 login_sessions
--------------------------------------------------------------------------------------
 {3dcdf650-6285-11e7-8bb1-091830ac5256: 13, 3dcdf651-6285-11e7-8bb1-091830ac5256: 18}

(1 rows)

```
We can also reference an individual item in the map by using its key.


## User-Defined Types

A UDT is scoped by the keyspace in which it is defined. We could have written CREATE TYPE my_keyspace.address. An user-defined data type is considered a collection, as its implementation is similar to a set, list, or map.

_Freezing is a concept that the Cassandra community has introduced as a forward compatibility mechanism. For now, you can nest a collection within another collection by marking it as frozen. In the future, when nested collections are fully supported, there will be a mechanism to “unfreeze” the nested collections, allowing the individual attributes to be accessed._

```
CREATE TYPE address (
    street text,
    city text,
    state text,
    zip_code int);

DESCRIBE KEYSPACE my_keyspace;

ALTER TABLE user ADD addresses map<text, frozen<address>>;

UPDATE user SET addresses = addresses +
 { 'home': { 
     street: '7712 E. Broadway', 
     city: 'Tucson',   
     state: 'AZ', 
     zip_code: 85715
} } 
WHERE first_name = 'Mary';

```

# Secondary Indexes
If you try to query on column in a Cassandra table that is not part of the primary key, you’ll soon realize that this is not allowed. We need to create a secondary index for the last_name column. A secondary index is an index on a column that is not part of the primary key.

```
SELECT * FROM user WHERE last_name = 'Nguyen';
InvalidRequest: code=2200 [Invalid query] message="No secondary indexes on the restricted columns support the provided operators: "


CREATE INDEX user_last_name_idx ON my_keyspace.user (last_name);
SELECT * FROM user WHERE last_name = 'Nguyen';

 first_name | addresses | emails | id   | last_name | login_sessions | phone_numbers | title
------------+-----------+--------+------+-----------+----------------+---------------+-------
       Bill |      null |   null | null |    Nguyen |           null |          null |   Mr.

(1 rows)
```

We’re not limited just to indexes based only on simple type columns. It’s also possible to create indexes that are based on values in collections. For maps in particular, we have the option of indexing either the keys (via the syntax KEYS(addresses)) or the values (which is the default), or both

```
CREATE INDEX ON user ( addresses );
CREATE INDEX ON user ( emails );
CREATE INDEX ON user ( phone_numbers );
```
to remove an index:
```
DROP INDEX user_last_name_idx;
```

Secondary indexes are not recommended for several specific cases:	
- Columns with high cardinality. For example, indexing on the user.addresses column could be very expensive, as the vast majority of addresses are unique.	
- Columns with very low data cardinality. For example, it would make little sense to index on the user.title column in order to support a query for every “Mrs.” in the user table, as this would result in a massive row in the index.	
- Columns that are frequently updated or deleted. Indexes built on these columns can generate errors if the amount of deleted data (tombstones) builds up more quickly than the compaction process can handle.
For optimal read performance, denormalized table designs or materialized views are generally preferred to using secondary indexes.


## Materialized Views

The PRIMARY KEY clause identifies the primary key for the materialized view, which must include all of the columns in the primary key of the base table. The WHERE clause provides support for filtering.Note that a filter must be specified for every primary key column of the materialized view (even if it is as simple as designating that the value IS NOT NULL.)

```
CREATE MATERIALIZED VIEW reservation.reservations_by_confirmation   
AS SELECT *   
FROM reservation.reservations_by_hotel_date  
WHERE confirm_number IS NOT NULL and hotel_id IS NOT NULL and 
start_date IS NOT NULL and room_number IS NOT NULL  
PRIMARY KEY (confirm_number, hotel_id, start_date, room_number);
```

# Data Modeling

```
CREATE KEYSPACE hotel
WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : 3};

CREATE TYPE hotel.address (
	street text,
	city text,
	state_or_province text,
	postal_code text,
	country text);CREATE TABLE hotel.hotels_by_poi (
	poi_name text,
	hotel_id text,
	name text,
	phone text,    address frozen<address>,
	PRIMARY KEY ((poi_name), hotel_id)) 
WITH comment = 'Q1. Find hotels near given poi'AND CLUSTERING ORDER BY (hotel_id ASC) ;

CREATE TABLE hotel.hotels (
	id text PRIMARY KEY,
	name text,
	phone text,
	address frozen<address>,
	pois set<text>) 
WITH comment = 'Q2. Find information about a hotel';

CREATE TABLE hotel.pois_by_hotel (
	poi_name text,
	hotel_id text,
	description text,
	PRIMARY KEY ((hotel_id), poi_name)) 
WITH comment = 'Q3. Find pois near a hotel';

CREATE TABLE hotel.available_rooms_by_hotel_date (
	hotel_id text,
	date date,
	room_number smallint,
	is_available boolean,
	PRIMARY KEY ((hotel_id), date, room_number)) 
WITH comment = 'Q4. Find available rooms by hotel / date';

CREATE TABLE hotel.amenities_by_room (
	hotel_id text,
	room_number smallint,
	amenity_name text,
	description text,
	PRIMARY KEY ((hotel_id, room_number), amenity_name)) 
WITH comment = 'Q5. Find amenities for a room';
```


```
CREATE KEYSPACE reservation
    WITH replication = {'class': 'SimpleStrategy', 'replication_factor' : 3};

CREATE TYPE reservation.address (
    street text,
    city text,
    state_or_province text,
    postal_code text,
    country text
);

CREATE TABLE reservation.reservations_by_hotel_date (
    hotel_id text,
    start_date date,
    end_date date,
    room_number smallint,
    confirm_number text,
    guest_id uuid,
    PRIMARY KEY ((hotel_id, start_date), room_number)
) WITH comment = 'Q7. Find reservations by hotel and date';

CREATE MATERIALIZED VIEW reservation.reservations_by_confirmation AS
    SELECT * FROM reservation.reservations_by_hotel_date
    WHERE confirm_number IS NOT NULL and hotel_id IS NOT NULL and
        start_date IS NOT NULL and room_number IS NOT NULL
    PRIMARY KEY (confirm_number, hotel_id, start_date, room_number);

CREATE TABLE reservation.reservations_by_guest (
    guest_last_name text,
    hotel_id text,
    start_date date,
    end_date date,
    room_number smallint,
    confirm_number text,
    guest_id uuid,
    PRIMARY KEY ((guest_last_name), hotel_id)
) WITH comment = 'Q8. Find reservations by guest name';

CREATE TABLE reservation.guests (
    guest_id uuid PRIMARY KEY,
    first_name text,
    last_name text,
    title text,
    emails set<text>,
    phone_numbers list<text>,
    addresses map<text, frozen<address>>,
    confirm_number text
) WITH comment = 'Q9. Find guest by ID';
```



