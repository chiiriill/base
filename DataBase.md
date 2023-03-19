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

### ! Сущность NULL никогда не бывает равной или не равной какой-либо другой сущности, даже самой себе


## Преобразования NULL в реальные значения

```sql
SELECT COALESCE(comm, 0) 
FROM EMP;
```

### Комментарий: COALESCE(какой столбец проверяем, во что преобразовываем)

## Поиск по шаблону(LIKE)

```sql
SELECT ename, job
FROM EMP
WHERE ename LIKE '%I%' OR job LIKE '%ER';
```

### LIKE:
- % - сколько угодно символов 
- _ - любой один символ

## Сортировка по подстрокам 

### Упорядочить эти строки по последним двум символам столбца должностей job

```sql
SELECT ename, job
FROM EMP
ORDER BY SUBSTR(job, LENGTH(job)-1);
```

### ! SUBSTR(строка, начальный символ)

## Действия при удалении записи главной таблицы

При удалении можно установить следующие опции:

- CASCADE: автоматически удаляет строки из зависимой таблицы при удалении  связанных строк в главной таблице.

- SET NULL: при удалении  связанной строки из главной таблицы устанавливает для столбца внешнего ключа значение NULL. (В этом случае столбец внешнего ключа должен поддерживать установку NULL).

- SET DEFAULT похоже на SET NULL за тем исключением, что значение  внешнего ключа устанавливается не в NULL, а в значение по умолчанию для данного столбца.

- RESTRICT: отклоняет удаление строк в главной таблице при наличии связанных строк в зависимой таблице.

### Важно! Если для столбца установлена опция SET NULL, то при его описании нельзя задать ограничение на пустое значение.

Пример

Ex:

```sql
CREATE TABLE book (
    book_id INT PRIMARY KEY AUTO_INCREMENT, 
    title VARCHAR(50), 
    author_id INT NOT NULL, 
    price DECIMAL(8,2), 
    amount INT, 
    FOREIGN KEY (author_id)  REFERENCES author (author_id) ON DELETE CASCADE
);
```