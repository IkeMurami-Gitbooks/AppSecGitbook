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
