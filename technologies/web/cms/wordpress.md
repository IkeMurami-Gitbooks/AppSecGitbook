# WordPress

wpscan (Ruby) - Сканирование WordPress сайтов на уязвимости. git: [https://github.com/wpscanteam/wpscan](https://github.com/wpscanteam/wpscan)\
Web Interface для всего этого: [https://github.com/cyc10n3/WPScan\_Web\_Interface](https://github.com/cyc10n3/WPScan\_Web\_Interface)

`wpscan --url https://domain.com`

Можно донастраивать какие-то API токены для получения доп информации — [https://wpvulndb.com/users/sign\_up](https://wpvulndb.com/users/sign\_up)

Токен добавляем через ключ `--api-token`

`-o output_file.txt -f [cli | json | ..]`

## Install

Поднимаем на DO дроплет из маркетплейса. Настраиваем домен и файерволл. Заходим по SSH и следуем инструкциям.&#x20;

На нашем домене будет наш сайт\
На domain/wp-admin — админка

## User enum

```
https://domain.com/wp-json/wp/v2/users

или через wpscan:
wpscan -e u1-100 --url <URL>
```

## XML-RPC

Если вернутся методы - хорошо, для нас

```
POST /xmlrpc.php HTTP/1.1
Host: example.domain
Content-Length: 135

<?xml version="1.0" encoding="utf-8"?> 
<methodCall> 
<methodName>system.listMethods</methodName> 
<params></params> 
</methodCall>
```

## All Methods

Здесь могут быть описаны методы CMS:&#x20;

```
https://domain.com/wp-json/
```
