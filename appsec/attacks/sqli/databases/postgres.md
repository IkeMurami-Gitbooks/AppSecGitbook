# postgres

Выведет quer, потому что $\[somestring]$ - это вроде кавычки

```
SELECT $test$quer$test$
```

Code Execution

Если есть права суперпользователя в базе, то можно исполнять код с помощью функций СУБД: `COPY TO` и `FROM PROGRAM`.

