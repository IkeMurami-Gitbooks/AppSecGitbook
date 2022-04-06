# Nginx

Слышал как-то об инструменте для тестирования конфигураций веб-сервера nginx на наличие уязвимостей, но не обратил на него внимания. Сейчас узнал, что этот инструмент имеет в репозитории хорошо описанные сценарии эксплуатации искомых уязвимостей и неплохую Вики:\
[https://github.com/yandex/gixy](https://github.com/yandex/gixy)

[https://github.com/wallarm/awesome-nginx-security](https://github.com/wallarm/awesome-nginx-security)

## Ошибки конфигурации

### Дефолтный конфиг

Пусть есть дефолтный конфиг nginx. Разработчик копирует его, называет example.com и настраивает на нем что-то (например, php). А default не удаляет. При обращении к example.com/index.php - отрабатывает страничка.&#x20;

При входе на любой не существующий виртуальный домен на сервере (меняем host), выведется дефолтная страничка (ее обработает дефолтная конфигурация сервера) - "Welcome to Nginx".&#x20;

Фишечка: можем сделать следующий запрос и увидеть сорцы:

```
GET /example.com/index.php
Host: localhost
```

### Один upstream

Иногда пускают через один upstream поток на два сервиса (например, два разных сервачка).

Допустим, есть два хоста: a.example.com и b.example.com. На a-хосте вход по сертам, на b-хосте - нет. Тогда, иногда, может получится такая штука:&#x20;

```
curl -H "Host: a.example.com" http://b.example.com -v -k
```

И мы попадаем на a.example.com без авторизации.

### Missing root location

```
server {
        root /etc/nginx;

        location /hello.txt {
                try_files $uri $uri/ =404;
                proxy_pass http://127.0.0.1:8080/;
        }
}
```

В этом конфиге не описано как обрабатывать `location /`, а только `location /hello.txt`. При обращении например к `/nginx.conf` будет прочтен файл `/etc/nginx/nginx.conf`.

### Off-by-Slash

```
server {
        listen 80 default_server;

        server_name _;

        location /static {
                alias /usr/share/nginx/static/;
        }

        location /api {
                proxy_pass http://apiserver/v1/;
        }
}
```

В этом конфиге `location /api` не закрыто слешем. Как происходиит обработка такого запроса:

```
http://apisesrver/api/user
```

 Сначала nginx нормализует URL (например: `http://apiserver/api/user/../user -> http://apiserver/api/user`). Затем ищет префикс `/api` и все, что после него, подставляет в `proxy_pass`:&#x20;

```
http://apisesrver/api/user -> http://apiserver/v1//user
```

Затем этот URL нормализуется.

Ну а если мы запросим вот такой адрес?)

```
http://apisesrver/api../server-status
```

Ответ: он уйдет на `http://apiserver/server-status`.

### Небезопасное использование переменных nginx

#### SCRIPT\_NAME

```
location ~ \.php$ {
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_pass 127.0.0.1:9000;
}
```

Здесь основная проблема в том, что PHP интерпретатору будет отправлен любой URL (если он оканчивается на php), даже если самого скрипта нет.&#x20;

Например: если обратиться `/v1/exploit_php.jpg/notexist.php`, то если не выставлены определенные флаги, fastcgi интерпретатор отбросит notexist.php и попытается обратиться к `exploit_php.jpg` и проинтерпретировать его как php скрипт.

Подробнее и больше примеров: [https://www.nginx.com/resources/wiki/start/topics/tutorials/config\_pitfalls/#passing-uncontrolled-requests-to-php](https://www.nginx.com/resources/wiki/start/topics/tutorials/config\_pitfalls/#passing-uncontrolled-requests-to-php)

### Обработка сырых данных от бэкенд сервера

Nginx в моде `proxy_pass` может скрывать ошибки и заголовки, приходящие с бэкенд-сервера. Это очень удобно. Nginx автоматом подсунет дефолтную страничку на ошибки. Но что, если nginx не поймет ответ сервера?

Если клиент отправит некорректный http запрос, то он будет переслан бэкенд-серверу как есть; сервер вернет raw ответ и Nginx его не сможет обработать и вернет его клиенту как есть. ТТаким образом можно увидеть какие-то необычные заголовки, данные и тп

Например:

Бэкенд приложение:

```python
def application(environ, start_response):
   start_response('500 Error', [('Content-Type',
'text/html'),('Secret-Header','secret-info')])
   return [b"Secret info, should not be visible!"]
```

Nginx конфиг:

```
http {
   error_page 500 /html/error.html;
   proxy_intercept_errors on;
   proxy_hide_header Secret-Header;
}
```

[proxy\_intercept\_errors](http://nginx.org/en/docs/http/ngx\_http\_proxy\_module.html#proxy\_intercept\_errors) вернет страницу ошибки error\_page, если код ответа сервера > 300

Если отправить нормальный GET запрос, то Nginx вернет:

```
HTTP/1.1 500 Internal Server Error
Server: nginx/1.10.3
Content-Type: text/html
Content-Length: 34
Connection: close
```

Но, если отправить некорректный запрос, такой как:

```
GET /? XTTP/1.1
Host: 127.0.0.1
Connection: close
```

То Nginx вернет следующий ответ:

```
XTTP/1.1 500 Error
Content-Type: text/html
Secret-Header: secret-info

Secret info, should not be visible!
```

### Path Traversal

Если директива в конфиге Nginx `merge_slashes` включена (`on`), то Nginx будет убирать лишние слэши, например `/// -> /`, `///../../ -> /../../`.&#x20;

Если директива выключена, то возможны случаи, когда:

```
/../index.js -> 404
///../index.js -> 200

Потому что в первом случае бэкенд серверу уйдет index.js, а во втором
//../index.js
```

Подробнее: [https://medium.com/appsflyer/nginx-may-be-protecting-your-applications-from-traversal-attacks-without-you-even-knowing-b08f882fd43d](https://medium.com/appsflyer/nginx-may-be-protecting-your-applications-from-traversal-attacks-without-you-even-knowing-b08f882fd43d)
