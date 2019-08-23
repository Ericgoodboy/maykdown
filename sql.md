# sql
- sql中应该让in中的东西尽量少，能用between的不要用in
- select 务必明确字段名，减少内存和带宽的使用
- 当只需要一条数据的时候要用limit 1
- 如果排序字段没有用到索引尽量少用排序
- 如果限制条件中其他字段没有索引，尽量少用or
     ```
     or两边的字段中，如果有一个不是索引字段，而其他条件也不是引字段，会造成该查询不走索引的情况。很多时候使用union al或者是union（必要的时候）的方式来代替“or”会得到更好的果。
     ```
- 劲量用union all 代替union
- 不要使用order  by rand()
  ```
  select id from `dynamic` order by rand() limit 1000;
  ```
- 区分in和exists、not in和not exists
  ```
  select * from 表A where id in (select id from 表B)
  ```
    相当于：
    ```
    select * from 表A where exists(select * from 表B where 表B.id=表A.id)
    ```
    区分in和exists主要是造成了驱动顺序的改变（这是性能变化的关键），如果是exists，那么以外层表为驱动表，先被访问，如果是IN，那么先执行子查询。所以IN适合于外表大而内表小的情况；EXISTS适合于外表小而内表大的情况。

    关于not in和not exists，推荐使用not exists，不仅仅是效率问题，not in可能存在逻辑问题。如何高效的写出一个替代not exists的SQL语句？

    原SQL语句：
    ```
    select colname … from A表 where a.id not in (select b.id from B表)
    ```
    高效的SQL语句：
    ```
    select colname … from A表 Left join B表 on where a.id = b.id where b.id is null
    ```
- 10、使用合理的分页方式以提高分页的效率
  原始SQL：
  ```
  select id,name from product limit 866613, 20
  ```
  高效SQL：
  ```
  select id,name from product where id> 866612 limit 20
  ```
- 分段查询：
- 避免在where子句中对字段进行null值判断
- 不建议使用%前缀模糊查询（可以使用全文索引）
- 避免在where子句中对字段进行表达式操做

    比如：
    ```
    select user_id,user_project from user_base where age*2=36;
    ```
- 多用inner join 少用left join



