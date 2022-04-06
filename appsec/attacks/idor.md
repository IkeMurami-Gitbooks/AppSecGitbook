---
description: Testing for IDOR/Broken object level authorization
---

# IDOR

## Поиск векторов для IDOR

1. Что используется для аутентификации? JWT, API ключи, куки, токены? \
   Пробуем заменять токены в запросах с высокими  привилегиями, на токены учеток с более низкими привилегиями. Смотрим, как меняется ответ сервера.
2. Как исполльзуются идентификаторы, хеши и API в целом. Если есть доступ к документации - круто!

Каждый раз, когда приходит новый API с идентификаторами, спрашиваем себя:

1. Идентификатор принадлежит закрытому ресурсу? (/api/user/123/news или /api/user/123/transaction)
2. Какие идентификаторы принадлежат пользователю?
3. Какие роли есть в API?

## Чейны с IDOR (идеи)

1. IDOR - сама по себе может быть слабой уязвимостью. Если связать ее с XSS - то мы можем получить Stored XSS (пример?)
2. Если IDOR на эндпоинте с UUID, свяжите с Information Disclosure
3. Ну и опять же, проявлять творческих подход)

## Пример обхода проверок

### Добавление параметров

```
GET /api_v1/messages                     --> 401
vs 
GET /api_v1/messages?user_id=victim_uuid --> 200
```

### HTTP Parameter pollution

```
GET /api_v1/messages?user_id=VICTIM_ID                     --> 401 Unauthorized
GET /api_v1/messages?user_id=ATTACKER_ID&user_id=VICTIM_ID --> 200 OK


GET /api_v1/messages?user_id=YOUR_USER_ID[]&user_id=ANOTHER_USERS_ID[]
```

### Добавление .json к пути, если у нас Ruby

```
/user_data/2341      --> 401 Unauthorized
/user_data/2341.json --> 200 OK
```

### Пробовать на других версиях API

```
/v3/users_data/1234 --> 403 Forbidden
/v1/users_data/1234 --> 200 OK
```

### Оборачивание ID в массив

```
{“id”:111}   --> 401 Unauthriozied
{“id”:[111]} --> 200 OK
```

### Оборачивание ID в JSON объект

```
{“id”:111}        --> 401 Unauthriozied

{“id”:{“id”:111}} --> 200 OK
```

### JSON Parameter Pollution

```
POST /api/get_profile
Content-Type: application/json

{“user_id”:<legit_id>,”user_id”:<victim’s_id>}
```

## Другие приемы

* Пробовать отправлять wildcard (\*) вместо ID. Это старый прием, но иногда работает.
* Если ID - число, обязательно попробовать побрутать, а не просто угадывать
* Если путь содержит имена, например: /api/users/myinfo, то стоит попробовать: /api/admins/myinfo
* Пробовать разные методы - GET/POST/PUT
* Использовать Burp расширение - autorize
* Если ничего не работает из этого - проявите творческих подход)

