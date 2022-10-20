# PHP

## Как выглядят сериализованные данные в PHP

```php
$user->name = "carlos";
$user->isLoggedIn = true;
```

Функции, которые ищем в коде:

```php
serialize()
unserialize()
```

Serialized:

```
O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}
```

## Tricks

Based on [PortSwigger Lab](https://portswigger.net/web-security/deserialization/exploiting).

* Можем менять значения
* Можем менять типы данных (например, если используется сравнение без учета типа данных — `$GET["password"] == $password` — можем через смену типов получать неожиданные резулльтаты, например, для PHP это `true`: `0 == "test"`)
* Данные из десериализованного значения может некорректно использоваться дальше (привет, SQLi, Path Traversal, ...)
*
