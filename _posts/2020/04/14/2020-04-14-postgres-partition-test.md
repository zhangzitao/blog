Partition is seperated by a field key. But this field will **not** generate index on each partition table.
We should create index manually.

This example show:
- use ```create_time``` as partition key
- create index of ```id``` and ```create_time```
- explain query if the index exist or not

```sql
DROP TABLE try;

CREATE TABLE try (
	id bigint NOT NULL,
	country varchar(255),
	create_time bigint NOT NULL
)
PARTITION BY RANGE (create_time);

CREATE INDEX idx_id ON try(id);
CREATE INDEX idx_create_time ON try(create_time);

CREATE TABLE sub_table_1 PARTITION OF try
FOR VALUES FROM (0) TO (10000);
CREATE TABLE sub_table_2 PARTITION OF try
FOR VALUES FROM (10000) TO (20000);

INSERT INTO try (id, country, create_time) select i,left(md5(i::text), 10),random()*19999 from generate_series(1,100000)  t(i);


EXPLAIN(ANALYZE,VERBOSE,BUFFERS)
SELECT * FROM try where id = 13;
/* 
Append  (cost=0.29..16.62 rows=2 width=27) (actual time=0.007..0.252 rows=1 loops=1)
  Buffers: shared hit=5
  ->  Index Scan using sub_table_1_id_idx on public.sub_table_1  (cost=0.29..8.31 rows=1 width=27) (actual time=0.006..0.007 rows=1 loops=1)
        Output: sub_table_1.id, sub_table_1.country, sub_table_1.create_time
        Index Cond: (sub_table_1.id = 13)
        Buffers: shared hit=3
  ->  Index Scan using sub_table_2_id_idx on public.sub_table_2  (cost=0.29..8.31 rows=1 width=27) (actual time=0.243..0.243 rows=0 loops=1)
        Output: sub_table_2.id, sub_table_2.country, sub_table_2.create_time
        Index Cond: (sub_table_2.id = 13)
        Buffers: shared hit=2
Planning Time: 10.959 ms
Execution Time: 0.271 ms
*/


EXPLAIN(ANALYZE,VERBOSE,BUFFERS)
SELECT * FROM try where create_time = 102;
/*
Append  (cost=4.33..22.67 rows=5 width=27) (actual time=0.263..0.500 rows=2 loops=1)
  Buffers: shared hit=2 read=2
  ->  Bitmap Heap Scan on public.sub_table_1  (cost=4.33..22.64 rows=5 width=27) (actual time=0.262..0.499 rows=2 loops=1)
        Output: sub_table_1.id, sub_table_1.country, sub_table_1.create_time
        Recheck Cond: (sub_table_1.create_time = 102)
        Heap Blocks: exact=2
        Buffers: shared hit=2 read=2
        ->  Bitmap Index Scan on sub_table_1_create_time_idx  (cost=0.00..4.33 rows=5 width=0) (actual time=0.009..0.009 rows=2 loops=1)
              Index Cond: (sub_table_1.create_time = 102)
              Buffers: shared hit=2
Planning Time: 2.233 ms
Execution Time: 0.821 ms
*/



DROP INDEX idx_id;
EXPLAIN(ANALYZE,VERBOSE,BUFFERS)
SELECT * FROM try where id = 13;
/*
Append  (cost=0.00..1986.01 rows=2 width=27) (actual time=0.322..6.616 rows=1 loops=1)
  Buffers: shared hit=407 read=329
  ->  Seq Scan on public.sub_table_1  (cost=0.00..993.53 rows=1 width=27) (actual time=0.322..3.435 rows=1 loops=1)
        Output: sub_table_1.id, sub_table_1.country, sub_table_1.create_time
        Filter: (sub_table_1.id = 13)
        Rows Removed by Filter: 50041
        Buffers: shared hit=221 read=147
  ->  Seq Scan on public.sub_table_2  (cost=0.00..992.48 rows=1 width=27) (actual time=3.179..3.179 rows=0 loops=1)
        Output: sub_table_2.id, sub_table_2.country, sub_table_2.create_time
        Filter: (sub_table_2.id = 13)
        Rows Removed by Filter: 49958
        Buffers: shared hit=186 read=182
Planning Time: 0.803 ms
Execution Time: 6.632 ms
*/


DROP INDEX idx_create_time;
EXPLAIN(ANALYZE,VERBOSE,BUFFERS)
SELECT * FROM try where create_time = 102;
/*
Append  (cost=0.00..993.55 rows=5 width=27) (actual time=0.851..9.165 rows=2 loops=1)
  Buffers: shared hit=157 read=211
  ->  Seq Scan on public.sub_table_1  (cost=0.00..993.53 rows=5 width=27) (actual time=0.850..9.163 rows=2 loops=1)
        Output: sub_table_1.id, sub_table_1.country, sub_table_1.create_time
        Filter: (sub_table_1.create_time = 102)
        Rows Removed by Filter: 50040
        Buffers: shared hit=157 read=211
Planning Time: 3.065 ms
Execution Time: 9.179 ms
*/

```
