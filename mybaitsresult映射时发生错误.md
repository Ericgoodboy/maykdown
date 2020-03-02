## mybaits报不能将一种类型映射成另一种类型的错误

错误如下

```java
org.springframework.dao.DataIntegrityViolationException: Error attempting to get column 'update_time' from result set.  Cause: java.sql.SQLDataException: Unsupported conversion from TIMESTAMP to java.lang.Integer
; Unsupported conversion from TIMESTAMP to java.lang.Integer; nested exception is java.sql.SQLDataException: Unsupported conversion from TIMESTAMP to java.lang.Integer

```

按照常规解bug的思路，简单分析一下，这可能是一个再做类型映射的时候除了错误，java中我用的是Date类型而数据库中我用的是timestamp类型，我去数据库中吧这个属性的类型改成datetime,然并卵，

尝试吧resultType改成hashMap,查看发现输出的结果没什么问题，结果也是Date类型的，所以可以锁定错误出现在数据转换成bean的过程中

对比之前写的bean，发现这个bean应为测试方便，吧构造函数覆盖了，让bean的构造除了问题

加上默认构造函数OK√