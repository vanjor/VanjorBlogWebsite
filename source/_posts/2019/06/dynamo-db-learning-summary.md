---
title: Learn on AWS DynamoDB  
date: 2019-06-25 22:00:00
tags:
  - DynamoDB
  - AWS
categories: 知识积累
---

## Basic Concept
* Tables: same with RDS
* Items: similar as record/row in RDS
* Attributes: similarcolumns in RDS

## Feature 
* A key-value and document database
* Fully cloud host, auto scale 
* No connection pool, only concurrent concept to avoid break limit for Dynamodb API is based on web service.

<!-- more -->

## Structure
* Table: A table is a collection of data
* Item: Similar to row in RDB, Each table contains zero or more items. An item is a group of attributes that is uniquely identifiable among all of the other items.
* Attribute: Each item is composed of one or more attributes. An attribute is a fundamental data element. Similar to column in RDB, each item contains multiple attributes.

## Keys
* Primary key (aka: Primary Index)
* Secondary Index
  * Local Secondary Indexes (LSI)
  * Global Secondary Indexe (GSI)

### Primary key
Table must define the primary key, support two types:
* partition key:
  * aka: hash attribute
  * should be unique
  * dynamo will run hash function on this to decide partition for physical storage.
* partition key and sort key:
  * a composite key with two attributes
  * aka: hash attribute + range attribute
  * partition key + sort key should be unique, while partition key can be not unique.
  * All items with the same partition key value are stored together, in sorted order by sort key.

More introduction:
* The only data types allowed for primary key attributes are string, number, or binary

### Secondary Index
DynamoDB supports two kinds of indexes:
* Global secondary index – An index with a partition key and sort key that can be different from table primary key. support eventually consistent or strongly consistent reads.
* Local secondary index – An index that has the same partition key with table primary key, but have a different sort key. only support eventually consistent reads.

Each table in DynamoDB has a limit of 20 global secondary indexes (default limit) and 5 local secondary indexes per table.

## Data Type
* Scalar Types: Number(int, long, double, float), String, Boolean, Bytes, Date.
* Collection types: Set
* Support any user defined type by using DynamoDBTypeConverter.
   
## Retrieve Operation
* Query: must specify a partition key value; the sort key is optional.
* Scan: lower efficient.

##  Query
* A Query operation always returns a result set. will be empty Set if not found.
* Query results are always sorted by the sort key value. By default, the sort order is ascending. We can set the ScanIndexForward parameter to false to reverse order.
* A single Query operation can retrieve a maximum of 1 MB of data. This limit applies before any FilterExpression is applied to the results. If LastEvaluatedKey is present in the response and is non-null, we will need to paginate the result set.

## Reference
* AWS official document: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html
* AWS DDB data types: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html
* DDB study notes:  https://rickhw.github.io/2016/08/17/AWS/Study-Notes-DynamoDB/
