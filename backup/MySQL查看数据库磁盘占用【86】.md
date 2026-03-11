在MySQL中，可以通过**SQL查询**或**命令行工具**快速查看数据库及表的磁盘占用情况，这对于容量规划和性能优化非常重要。

使用SQL查询

这是最常用且无需访问服务器文件系统的方法，依赖 *information_schema.tables* 提供的元数据。

**查看所有数据库大小：**

```sql
SELECT
   table_schema AS 'Database',
   ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
GROUP BY table_schema
ORDER BY `Size (MB)` DESC;
```

**查看单个数据库各表大小：**

```sql
SELECT
   table_name AS 'Table',
   ROUND((data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
WHERE table_schema = 'your_database_name'
ORDER BY `Size (MB)` DESC;
```

将 *your_database_name* 替换为实际库名即可。

使用命令行工具

如果有服务器访问权限，可以直接查看MySQL数据目录（默认 */var/lib/mysql*）的文件大小。

**查看所有数据库总占用：**

```sal
du -sh /var/lib/mysql
```

**查看指定数据库占用：**

```sql
du -sh /var/lib/mysql/your_database_name
```


这种方式能反映真实磁盘占用，包括未释放的空间。

验证与优化建议

- 对比SQL查询结果与*du*命令结果，确认是否存在大量碎片或未释放空间。

- 定期执行：

  ```sql
  OPTIMIZE TABLE table_name;
  ```


可回收碎片空间，但会锁表，建议低峰期执行且不宜频繁。

- 删除无用数据、归档历史数据、启用压缩存储，可进一步降低占用。

通过以上方法，可以全面掌握MySQL数据库的磁盘使用情况，并结合优化手段保持存储高效。