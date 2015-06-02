# Database

## Index
### Clustered vs Non Clustered Index
*  A clustered index: physically arrange rows in the same order as the index.
*  Non clustered index: a list that has pointers to the physical rows.
*  A table can be only one clustered index.
*  Writing to a table with a clustered index can be slower, if there is a need to rearrange the data.


## View
### View vs Materialized View
*  Materialized views are disk based and are updated periodically based upon the query definition.
*  Views are virtual only and run the query definition each time they are accessed.
