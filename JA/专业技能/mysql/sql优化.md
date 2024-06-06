
## insert
批量插入
	大批量插入 load  --local-infile
手动提交事务

主键顺序插入
	乱序插入可能出现 页分裂
	页合并

## order by
Using filesort 
	通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作
	排序缓冲区大小默认256k
Using index 
	通过有序索引顺序扫描直接返回有序数据
索引创建时可以指定升序asc和降序desc
尽量使用覆盖索引

## group by


## limit by
limit 200000000,10 查询代价大
覆盖索引+子查询
	不支持子查询里面有limit
```sql
select id from table1 order by id limit 9000000000,10;
- 不支持这种含limit的子查询
select * from table1 where id in (select id from table1 order by id limit 9000000000,10;)

select *  from table1 t,(select id from table1 order by id limit 9000000000,10) a where t.id = a.id;
```



## count 
MyISAM 引擎把一个表的总行数存在了磁盘上，直接返回，没有where等条件
InnoDB引擎会一行一行读，累计计数
优化思路
	自己计数
![[Pasted image 20240330110934.png]]


## update
innoDB  事务、外键、行锁
按索引 行锁
不按索引或者索引失效 行锁升级为表锁
