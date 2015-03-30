#Paginated Queries in Oracle

By narrowing queries, user can select a more manageable amount of data.

```SQL
select * 
  from 
( select rownum rnum, a.*
    from (your_query) a
   where rownum <= :M )
where rnum >= :N;
```