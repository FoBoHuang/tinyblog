# tinyblog 开发中常用命令

## Dump 表结构

```bash
mysqldump -h127.0.0.1 -utinyblog --databases tinyblog -p'tinyblog1234' --add-drop-database --add-drop-table --add-drop-trigger --add-locks --no-data > /tmp/tinyblog.sql
```
