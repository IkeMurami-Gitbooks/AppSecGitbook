# MIME type bypass & filter evasions

Firefox пытается угадать тип файла, если в заголовке Content-Type нет символа /. Причем X-Content-Type-Options: nosniff в данном случае никак не влияет, тк используется Firefox только при попытке подключить файлы с некорректным content-type в

```javascript
<script src=> и <link rel="stylesheet" href=>
```

Примеры

[https://blog.blackfan.ru/2020/03/volgactf-2020-qualifier-writeup.html](https://blog.blackfan.ru/2020/03/volgactf-2020-qualifier-writeup.html)

Еще ресерч по Content-Type: [https://github.com/BlackFan/content-type-research](https://github.com/BlackFan/content-type-research)



