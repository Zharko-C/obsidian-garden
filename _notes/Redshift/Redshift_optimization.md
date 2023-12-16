---
{"dg-publish":true,"permalink":"/study/data/data-warehouse/redshift/redshift-optimization/","tags":["gardenEntry"]}
---

## Distribution Style

Redshift is comprised of nodes hosted in separate disks. The way that data is divided across these nodes is referred to as the Data Distribution Style.

There are 4 distribution styles.

 - EVEN
 - KEY
 - ALL
 - AUTO

### EVEN

Default dist style for tables not expected to use the table in joins.

Example of using this dist style:
```
create table sample
(
id int,
name varchar(100),
age int
)
DISTSTYLE EVEN;
```

### KEY

Data is distributed according to the values of one column. If multiple columns are distributed with the same KEY column, it means that the tables are collocated. Collocated tables are able to join much more efficiently.
Use KEY dist style when expecting to be joining tables on a predefined column.

Example of using this dist style:
```
create table sample
(
id int,
name varchar(100),
age int
)
DISTSTYLE KEY
DISTKEY(ID);
```

### ALL

Data is copied to each node. This allows every table to be collocated, optimal for joins. It comes with the downside of slow loading. Only advised for small databases.

Example of using this dist style:
```
create table sample
(
id int,
name varchar(100),
age int
)
DISTSTYLE ALL;
```

## AUTO

AUTO is the default dist style if none other is selected. Redshift will set this based on the size of the table and the availability of a suitable distribution key.
By default on small tables Redshift will assign the ALL dist style, when the table grows larger it might assign the KEY dist style using the primary key as a dist key. Once the table grows larger or there is no obvious key available the dist style switches to EVEN.


Example of using this dist style:
It's the default, it's assigned if you don't add anything.
```
create table sample
(
id int,
name varchar(100),
age int
);
