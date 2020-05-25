---
title: LOAD DATA
summary: TiDB 数据库中 LOAD DATA 的使用概况。
category: reference
aliases: ['/docs-cn/dev/reference/sql/statements/load-data/']
---

# LOAD DATA

`LOAD DATA` 语句用于将数据批量加载到 TiDB 表中。

## 语法图

**LoadDataStmt:**

![LoadDataStmt](/media/sqlgram/LoadDataStmt.png)

## 参数说明

用户可以使用 `FIELDS` 参数来指定如何处理数据格式，使用 `FIELDS TERMINATED BY` 来指定每个数据的分隔符号，使用 `FIELDS ENCLOSED BY` 来指定消除数据的包围符号。如果用户希望以某个字符为结尾切分每行数据，可以使用 `LINES TERMINATED BY` 来指定行的终止符。

例如对于以下格式的数据：

```
"bob","20","street 1"\r\n
"alice","33","street 1"\r\n
```

如果想分别提取 `bob`、`20`、`street 1`，可以指定数据的分隔符号为 `','`，数据的包围符号为 `'\"'`。
可以写成：

```sql
FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\r\n'
```

如果不指定处理数据的参数，将会按以下参数处理

```sql
FIELDS TERMINATED BY '\t' ENCLOSED BY ''
LINES TERMINATED BY '\n'
```

用户可以通过 `IGNORE number LINES` 参数来忽略文件开始的 `number` 行，例如可以使用 `IGNORE 1 LINES` 来忽略文件的首行。

## 示例

{{< copyable "sql" >}}

```sql
CREATE TABLE trips (
    ->  trip_id bigint NOT NULL PRIMARY KEY AUTO_INCREMENT,
    ->  duration integer not null,
    ->  start_date datetime,
    ->  end_date datetime,
    ->  start_station_number integer,
    ->  start_station varchar(255),
    ->  end_station_number integer,
    ->  end_station varchar(255),
    ->  bike_number varchar(255),
    ->  member_type varchar(255)
    -> );

INSERT INTO trips VALUES (1, "1012","2010-09-20 11:27:04","2010-09-20 11:43:56","31208","M St & New Jersey Ave SE","31108","4th & M St SW","W00742","Member");
INSERT INTO trips VALUES (2, "61","2010-09-20 11:41:22","2010-09-20 11:42:23","31209","1st & N St  SE","31209","1st & N St  SE","W00032","Member");
INSERT INTO trips VALUES (3, "2690","2010-09-20 12:05:37","2010-09-20 12:50:27","31600","5th & K St NW","31100","19th St & Pennsylvania Ave NW","W00993","Member");
```

```
Query OK, 0 rows affected (0.14 sec)
```

然后通过 `SELECT ... INTO OUTFILE` 将其导出到文件：

{{< copyable "sql" >}}

```sql
SELECT * FROM trips into outfile '/tmp/trips.csv';
```

```
Query OK, 0 rows affected (0.00 sec)
```

得到的文件内容为：
```
1       1012    2010-09-20 11:27:04     2010-09-20 11:43:56     31208   M St & New Jersey Ave SE        31108   4th & M St SW   W00742  Member
2       61      2010-09-20 11:41:22     2010-09-20 11:42:23     31209   1st & N St  SE  31209   1st & N St  SE  W00032  Member
3       2690    2010-09-20 12:05:37     2010-09-20 12:50:27     31600   5th & K St NW   31100   19th St & Pennsylvania Ave NW   W00993  Member
```


## 另请参阅

* [LOAD](/sql-statements/sql-statement-insert.md)
* [乐观事务模型](/optimistic-transaction.md)
