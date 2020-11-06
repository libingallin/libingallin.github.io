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
FROM
  (SELECT id,
          col,
          row_number() over(partition BY col ORDER BY time DESC) AS rank
   FROM db.table
   ...) AS t
WHERE t.rank = 1
...
```

## 4. `WITH.. AS` 语句

```sql
WITH alias_1 AS (
  SELECT xx,
         yy,
         zz
  FROM table_1
  ...
)
SELECT * FROM alias_1
```

Or

```sql
WITH alias_1 AS (
  SELECT xx,
         yy,
         zz
  FROM table_1
  ...
),
alias_2 AS (
  SELECT xxx,
         yyy,
         zzz
  FROM table_2
  ...
)
SELECT * FROM alias_1
LEFT JOIN alias_2 ON alias_1.xx = alias_2.xxx
```

## 5. 正则表达式
```sql
SELECT xxx,
       yyy,
       IF(zzz REGEXP '[买断|拍卖|公务车|车商回购|法律诉讼]', 1, 0) AS zzz_2
FROM table
...
```
