# HTTP Smuggling

## About

Атака, при которой фронт (front-end proxy, HTTP-enabled firewall или цепочка серверов с различной конфигурацией) воспринимает запрос как один (например, он открывает сокет с бэкенд сервером и в рамках одного сокета шлет запросы пользователей), а на бэке он распадается на два (бэкенд по специальным заголовкам понимает, где конец одного запроса и начало следующего).

![](../../.gitbook/assets/изображение.png)

Делается это с помощью заголовков **Content-Length (CL)** или **Transfer-Encoding (TE)**.

## Как это происходит?

Спецификация HTTP позволяет указать серверу, что запрос завершен, двумя способами. Использовать **Content-Length** или **Transfer-Encoding: chunked**.

**Content-Length** указывает сколько байтов тело запроса. Тогда как **Transfer-Encodiing: chunked** указывает, что тело запроса будет отправлено кусками, разделенными последовательностями новой строки, причем каждому фрагменту предшествует его размер в байтах (hexadecimal). Тело запроса заканчивается фрагментом нулевой длины.

Спецификация HTTP указывает, что при наличии CL и TE заголовков, **CL следует игнорировать**. Однако, по разным причинам при отправке обоих этих заголовков в одном запросе может возникнуть конфликты:

* Некоторые HTTP сервера не поддерживают TE заголовок
* Другие могут его игнорировать при использовании некоторых кодирований/обфускации

### Направления проверок

* CL.TE: фронт использует CL, бэк — TE
* TE.CL: фронт использует TE, бэк — CL
* TE.TE: и фронт и бэк используют TE, но они по разному реагируют на обфускацию заголовков

### CL.TE

Пример запроса

```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```

В PortSwigger есть лаба: [https://portswigger.net/web-security/request-smuggling/lab-basic-cl-te](https://portswigger.net/web-security/request-smuggling/lab-basic-cl-te)

### TE.CL

Пример запроса

```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked

8
SMUGGLED
0


```

В PortSwigger есть лаба: [https://portswigger.net/web-security/request-smuggling/lab-basic-te-cl](https://portswigger.net/web-security/request-smuggling/lab-basic-te-cl)\
Мое решение: [https://gist.github.com/IkeMurami/45abe13216559cfb89d3b1397a53fd02](https://gist.github.com/IkeMurami/45abe13216559cfb89d3b1397a53fd02)

### TE.TE

Как указано выше: оба сервера поддерживают заголовок TE, но если он обфусцирован немного, могут на него среагировать по разному. Наша задача: найти такую обфускацию, при которой один сервер обработает заголовок, другой — проигнорирует. Примеры обфускации заголовка:

```
Transfer-Encoding: xchunked

Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked

[space]Transfer-Encoding: chunked

X: X[\n]Transfer-Encoding: chunked

Transfer-Encoding
: chunked
```

### 2xCL

Использование двух `Content-Length` — где-то может взяться первый `Content-Length`, а на другом сервере — второй

```
POST / HTTP/1.1
Host: example.com
Content-Length: 6
Content-Length: 5

12345GPOST / HTTP/1.1
Host: example.com
…
```

Тогда с первого вернется: UNKNOWN HTTP Method GPOST (как детект)

Подробнее: [https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn](https://portswigger.net/research/http-desync-attacks-request-smuggling-reborn)

### Еще трюк

I found another way to do HTTP smuggling, you can use T-E: chunKed\
K is the Kelvin symbol (%E2%84%AA) If the header is converted to lowercase, you get 'chunked' in ascii, if it's converted to uppercase it will stay the same (invalid)

## Как детектировать

Smuggler extension for Burp: [https://kalilinuxtutorials.com/http-request-smuggler-extension-burp-suite/](https://kalilinuxtutorials.com/http-request-smuggler-extension-burp-suite/)

Python script: [https://github.com/gwen001/pentest-tools/blob/master/smuggler.py](https://github.com/gwen001/pentest-tools/blob/master/smuggler.py)

## TODO

Сделать лабу на HTTP Smuggling

## Mitigation

* Убедитесь, что одно и то же серверное программное обеспечение используется как на внешнем, так и на внутреннем серверах, чтобы они синхронизировались, какой заголовок они будут использовать, чтобы предотвратить конфликты (либо **Content-Length**, либо **Transfer Encoding: chunked**).&#x20;
* Некоторые провайдеры WAF уже имеют встроенные средства защиты при обнаружении аномальных запросов. Уточните у своего провайдера, есть ли у него поддержка для этого.&#x20;
* Отключите повторное использование внутренних соединений, чтобы каждый внутренний запрос отправлялся через отдельное сетевое соединение.

## Papers

Статьи: \
[https://habr.com/ru/post/468489/](https://habr.com/ru/post/468489/)\
[https://blog.deteact.com/gunicorn-http-request-smuggling/](https://blog.deteact.com/gunicorn-http-request-smuggling/)\
[https://portswigger.net/web-security/request-smuggling](https://portswigger.net/web-security/request-smuggling)\
[https://www.rcesecurity.com/2020/11/Smuggling-an-un-exploitable-xss/](https://www.rcesecurity.com/2020/11/Smuggling-an-un-exploitable-xss/)

## Labs

HTTP Smuggling lab: здесь внутри и на сокетах и в разных конфигурациях и тп [https://github.com/ZeddYu/HTTP-Smuggling-Lab](https://github.com/ZeddYu/HTTP-Smuggling-Lab)

И норм материал к этому: [https://regilero.github.io/english/security/2019/10/17/security\_apache\_traffic\_server\_http\_smuggling/](https://regilero.github.io/english/security/2019/10/17/security\_apache\_traffic\_server\_http\_smuggling/)

Мое скрипт по решению этих лаб: [https://github.com/IkeMurami/zscanutils/blob/main/scripts/http\_request\_over\_sockets.py](https://github.com/IkeMurami/zscanutils/blob/main/scripts/http\_request\_over\_sockets.py)

