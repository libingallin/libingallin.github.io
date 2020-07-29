# SQL 白痴的笔记

## 1. SQL 中的 value_counts
```sql
SELECT type, count(*)
FROM table
GROUP BY type
```

## 2. 使用中文 alias

```sql
SELECT aa AS `中文名`
FROM table
```
