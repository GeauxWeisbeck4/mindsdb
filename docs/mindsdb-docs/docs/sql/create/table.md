# `#!sql CREATE TABLE` Statement

## Description

The `#!sql CREATE TABLE` is used to create a table and fill it with the result of a subselect, usually used to materialize predictions into tables.

## Syntax

```sql
CREATE [{REPLACE}] TABLE [integration_name].[table_name]
    [SELECT ...]
```

It performs a subselect `#!sql [SELECT ...]` and gets data from it, thereafter it creates a table `#!sql [table_name]` in `#!sql [integration_name]`. lastly it performs an `#!sql INSERT INTO [integration_name].[table_name]` with the contents of the `#!sql [SELECT ...]`

!!!warning "`#!sql REPLACE`"
    If `#!sql REPLACE` is indicated then `#!sql [integration_name].[table_name]` will be **Dropped**

## Example

In this example we want to persist the predictions into a table `#!sql int1.tbl1`. Given the following schema:

```bash
int1
└── tbl1
mindsdb
└── predictor_name
int2
└── tbl2
```

Where:

|                  | Description                                                |
| ---------------- | ---------------------------------------------------------- |
| `int1`           | Integration for the table to be created in                 |
| `tbl1`           | Table to be created                                        |
| `predictor_name` | Name of the model to be used                               |
| `int2`           | Database to be used as a source in the inner `#!sql SELECT` |
| `tbl2`           | Table to be used as a source.                               |

In order to achive the desired result we could execute the following query:

```sql
CREATE TABLE int1.tbl1 (
    SELECT *
    FROM int2.tbl2 AS ta
    JOIN mindsdb.predictor_name AS tb
    WHERE ta.date > '2015-12-31'
)
```