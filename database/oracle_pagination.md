# Paginated Queries in Oracle

By narrowing queries, user can select a more manageable amount of data.

```SQL
select * from 
( select rownum rnum, {SCHEMA_NAME}.*
    from ({QUERY}) {SCHEMA_NAME} -- include any ordering within {QUERY}
   where rownum <= {MAX_ROW} )
where rnum >= {MIN_ROW};
```

Note that rownum is a pseudocolumn provided by Oracle, calculated after the query predicate has been passed but before the query been processed and sorted. The above structure is smart enough to only select the specified rows without sorting the entire table. Row numbering starts at 1.