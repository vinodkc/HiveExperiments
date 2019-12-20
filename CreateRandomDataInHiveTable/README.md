# Create test data in hive table  
  
1) Login to hiveserver2 host node  
  
```beeline -u 'jdbc:hive2://c120-node2.squadron-labs.com:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2' -n hive  ```
  
```create database perf;  ```
  
```use perf;  ```
  
```CREATE  TABLE t1 (productID int, code string,name string,quantity int, price float) STORED AS parquet ;  ```
  
```show create table t1;  ```
  
  ```
+------------------------------------------------------------------------------+--+  
|                                createtab_stmt                                |  
+------------------------------------------------------------------------------+--+  
| CREATE TABLE `t1`(                                                           |  
|   `productid` int,                                                           |  
|   `code` string,                                                             |  
|   `name` string,                                                             |  
|   `quantity` int,                                                            |  
|   `price` float)                                                             |  
| ROW FORMAT SERDE                                                             |  
|   'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'              |  
| STORED AS INPUTFORMAT                                                        |  
|   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'            |  
| OUTPUTFORMAT                                                                 |  
|   'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'           |  
| LOCATION                                                                     |  
|   'hdfs://c120-node2.squadron-labs.com:8020/apps/hive/warehouse/perf.db/t1'  |  
| TBLPROPERTIES (                                                              |  
|   'COLUMN_STATS_ACCURATE'='{\"BASIC_STATS\":\"true\"}',                      |  
|   'numFiles'='0',                                                            |  
|   'numRows'='0',                                                             |  
|   'rawDataSize'='0',                                                         |  
|   'totalSize'='0',                                                           |  
|   'transient_lastDdlTime'='1576831447')                                      |  
+------------------------------------------------------------------------------+--+  
  ```
save above content to schemafile.txt  
  
  
```python hiveRandom  -s schemafile.txt  -n 10 -d perf  ```
 ``` 
[spark@c120-node2 hiveRandom-master]$ ll  
total 44  
-rw-r--r-- 1 spark hadoop   571 Dec 20 08:54 HiveRandom.csv  
-rw-r--r-- 1 spark hadoop  2019 Dec 20 08:54 HiveRandom.hql  
-rw-r--r-- 1 spark hadoop 11357 Nov 19  2018 LICENSE  
-rw-r--r-- 1 spark hadoop    68 Nov 19  2018 README.md  
-rwxr-xr-x 1 spark hadoop 14854 Nov 19  2018 hiveRandom  
-rw-r--r-- 1 spark hadoop  2034 Dec 20 08:49 schemafile.txt  
  ```
  ```
beeline -u 'jdbc:hive2://c120-node2.squadron-labs.com:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2' -n hive -f HiveRandom.hql  ```

  
```  
beeline -u 'jdbc:hive2://c120-node2.squadron-labs.com:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2' -n hive -e 'select count(*) from perf.t1'  ```
```
Connecting to jdbc:hive2://c120-node2.squadron-labs.com:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2  
Connected to: Apache Hive (version 1.2.1000.2.6.5.0-292)  
Driver: Hive JDBC (version 1.2.1000.2.6.5.0-292)  
Transaction isolation: TRANSACTION_REPEATABLE_READ  
+--------+--+  
|  _c0   |  
+--------+--+  
| 10045  |  
+--------+--+  
1 row selected (0.192 seconds)  
Beeline version 1.2.1000.2.6.5.0-292 by Apache Hive  
Closing: 0: jdbc:hive2://c120-node2.squadron-labs.com:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2```
