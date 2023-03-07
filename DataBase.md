# Database 

## Конкантенация значений столбцов

### Mysql:

```sql
SELECT CONCAT(ename, ' works as a ', job) AS msg
FROM EMP
WHERE deptno=10;
```

### PostgreSQL:

```sql
SELECT ename || ' works as a ' || job AS msg
FROM EMP
WHERE deptno=10;
```

## Использование case

```sql 
SELECT ename,sal, 
CASE WHEN sal <= 2000 THEN 'UNDEPRAID'
     WHEN sal >= 4000 THEN 'OVERPAID'
     ELSE 'OK'
     END AS status
FROM EMP;
```

## Ограничение вывода 

```sql
SELECT *
FROM EMP
LIMIT 5;
```

## Извлечение из таблицы произвольных записей

### Mysql:

```sql 
SELECT ename, job
FROM EMP
ORDER BY RAND() 
LIMIT 5;
```

### PostgreSQL:

```sql 
SELECT ename, job
FROM EMP
ORDER BY RANDOM() 
LIMIT 5;
```

## Поиск значений NULL

```sql
SELECT * 
FROM EMP
WHERE comm IS NULL;
```

### Сущность NULL никогда не бывает равной или не равной какой-либо другой сущности, 