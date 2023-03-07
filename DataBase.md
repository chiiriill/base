# Database 

## Конкантенация значений столбцов

### Mysql:

```sql
SELECT CONCAT(ename, ' works as a ', job) as msg
FROM EMP
WHERE deptno=10;
```

### Postgresql:

```sql
SELECT ename || ' works as a ' || job as msg
FROM EMP
WHERE deptno=10;
```

