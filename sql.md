# SQL

### Delete Duplicate Rows
```sql
delete from 
    table_name a
where 
    a.rowid > 
    any (select b.rowid
    from 
        table_name b
    where 
        a.col1 = b.col1
    and 
        a.col2 = b.col2
    )
;
```