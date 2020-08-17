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

## 3. 选择字段 col 相同的最新数据（根据 time）

```sql
SELECT id,
       col
FROM (SELECT id,
             col,
             row_number() over(partition BY col ORDER BY time DESC) AS rank
      FROM db.table
      WHERE xxx) AS t
WHERE t.rank = 1
```
