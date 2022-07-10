# Кодировки

## **Unicode transformation issues**

Вот такой прикол: и первое и второе один символ в UTF-8. Один может фильтроваться, а другой нет: такое можно использовать при различных байпасах.

![](<../../.gitbook/assets/изображение (8).png>)

![](<../../.gitbook/assets/изображение (9).png>)

## Арабские кодировки

Есть (полу)фишинговая атака: пишем сообщение, используя символ смены направления написания текста moc.live.example.com. При переходе по такой ссылке (или при неправильной работе с кодировками строк в коде) пользователь будет перенаправлен на moc.elpmaxe.evil.com.&#x20;

Это называется Unicode RTLO (RightToLeftOverride) problem: [https://xakep.ru/2022/03/29/rtlo-problems/](https://xakep.ru/2022/03/29/rtlo-problems/)

```markup
<html>
    <header>
    
    </header>
    <body>
        <div id="sploit">https://google.com/&#x202E;https://example.com/rtlo</div>
        <a href="#" onclick="CopyToClipboard('sample');return false;">Copy Sploit</a><br><br>
        <!-- Этот текст не должен разворачиваться (см как в телеге это отобразится) -->
        <div id="sample">&#x202E;1234</div>
        <a href="#" onclick="CopyToClipboard('sample');return false;">Copy Text</a>
        <script>
            function CopyToClipboard(id)
            {
                var r = document.createRange();
                r.selectNode(document.getElementById(id));
                window.getSelection().removeAllRanges();
                window.getSelection().addRange(r);
                document.execCommand('copy');
                window.getSelection().removeAllRanges();
            }
            </script>
    </body>
</html>
```
