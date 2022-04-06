# Search links

Поиск url и ajax запросов в js-скриптах\
JSParser [https://github.com/nahamsec/JSParser](https://github.com/nahamsec/JSParser)\
relative-url-extractor [https://github.com/jobertabma/relative-url-extractor](https://github.com/jobertabma/relative-url-extractor)

LinkFinder - ищем все endpoint and path [https://github.com/GerbenJavado/LinkFinder](https://github.com/GerbenJavado/LinkFinder)\
Главная регулярка:&#x20;

```
`(?:"|')(((?:[a-zA-Z]{1,10}://|//)[^"'/]{1,}\.[a-zA-Z]{2,}[^"']{0,})|((?:/|\.\./|\./)[^"'><,;| *()(%%$^/\\\[\]][^"'><,;|()]{1,})|([a-zA-Z0-9_\-/]{1,}/[a-zA-Z0-9_\-/]{1,}\.(?:[a-zA-Z]{1,4}|action)(?:[\?|#][^"|']{0,}|))|([a-zA-Z0-9_\-/]{1,}/[a-zA-Z0-9_\-/]{3,}(?:[\?|#][^"|']{0,}|))|([a-zA-Z0-9_\-]{1,}\.(?:php|asp|aspx|jsp|json|action|html|js|txt|xml)(?:[\?|#][^"|']{0,}|)))(?:"|')`
```

Extract links from .js files! Simple regex - `(https?://)?/?[{}a-z0-9A-Z.-]{2,}/[{}/a-z0-9A-Z.-]+`
