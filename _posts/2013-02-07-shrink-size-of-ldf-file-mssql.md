---
title: "Shrink size of .LDF file MSSQL"
date: 2013-02-07
categories: 
  - "technical"
tags: 
  - "delete-ldf-file"
  - "delete-sql-log"
  - "mssql-ldf-file"
  - "remove-sql-log-ms-sql"
  - "shrink-db-log-file-size"
  - "shrink-log"
  - "sql"
---

```sql
ALTER DATABASE databasename SET RECOVERY SIMPLE

DBCC SHRINKFILE (databasename_Log, 1)
```
