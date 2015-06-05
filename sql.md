# SQL

### Delete Duplicate Rows
1.  Use `any`

    ```sql
    DELETE
    FROM
        table_name a
    WHERE
        a.rowid > ANY ( SELECT b.rowid
                        FROM 
                            table_name b
                        WHERE 
                            a.col1 = b.col1
                        AND 
                            a.col2 = b.col2 )
    ;
    ```

2.  Use `NOT IN` and `MIN`

    ```sql
    DELETE
    FROM
        table_name a
    WHERE
        a.rowid
    NOT IN ( SELECT MIN(b.rowid)
             FROM table_name b
             GROUP BY col1, col2 )
    ;
    ```