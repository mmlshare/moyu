# MySql

## Performance Schema

`Performance Schema`默认是开启的，可以通过在启动时修改配置文件`my.cnf` 的`performance_schema`值来开启或者关闭该功能。

```properties
[mysqld]
performance_schema=ON
```

当`mysql`服务启动时，会去初始化`Performance Schema`,可以通过以下的命令来查看初始化结果。`ON`说明初始化成功，`OFF`说明发生了错误，具体信息需要查看服务日志。

```sql
mysql> show variables like 'performance_schema';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| performance_schema | ON    |
+--------------------+-------+

```

`Performance Schema`是一种存储引擎的实现，所以你能在`INFORMATION_SCHEMA.ENGINES`中看到它。

```sql
mysql> SELECT * FROM INFORMATION_SCHEMA.ENGINES  WHERE ENGINE='PERFORMANCE_SCHEMA';
+--------------------+---------+--------------------+--------------+------+------------+
| ENGINE             | SUPPORT | COMMENT            | TRANSACTIONS | XA   | SAVEPOINTS |
+--------------------+---------+--------------------+--------------+------+------------+
| PERFORMANCE_SCHEMA | YES     | Performance Schema | NO           | NO   | NO         |
+--------------------+---------+--------------------+--------------+------+------------+

```



