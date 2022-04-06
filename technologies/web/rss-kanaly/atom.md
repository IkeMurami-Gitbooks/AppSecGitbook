# Atom

## About

Atom - общее название двух связных веб-технологий: формат для описания ресурсов на  веб-сайтах и протокол для публикации ресурсов

Формат Atom основан на XML  и позволяет описывать наборы веб-ресурсов - например: новостные ленты, анонсы статей в блоге и тп.

Протокол решает те же задачи, что и RSS, но возник позже. Формат описан в [RFC 4287](https://tools.ietf.org/html/rfc4287) и сейчас активно поддерживается Google во многих ее проектах.

### Протокол публикации

Протокол публикации Atom (также AtomPub) основан на HTTP и позволяет создавать, изменять и удалять ресурсы, собранные в коллекции на веб-сайте. Содержимое коллекций описывается в формате Atom, а для управления им используются стандартные методы HTTP. Протокол описан в [RFC 5023](https://tools.ietf.org/html/rfc5023).

[GData](https://developers.google.com/gdata/?csw=1) - расширение Atom и RSS от Google.

### Пример ленты в формате Atom

```markup
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom">
  <title>Мой блог</title>
  <subtitle>Самый лучший блог на свете</subtitle>
  <link href="http://example.org/"/>
  <updated>2003-12-13T18:30:02Z</updated>
  <author>
    <name>Иван Петров</name>
    <email>petrov@example.com</email>
  </author>
  <id>urn:uuid:60a76c80-d399-11d9-b91C-0003939e0af6</id>
  <entry>
    <title>Фотографии из Африки</title>
    <link href="http://example.org/2003/12/13/atom03"/>
    <id>urn:uuid:1225c695-cfb8-4ebb-aaaa-80da344efa6a</id>
    <updated>2003-12-13T18:30:02Z</updated>
    <summary>Я вернулся из Африки и выложил свои фотографии...</summary>
  </entry>
</feed>
```

## Как парсить

```python
# pip3 install atoma

import atoma, requests

response = requests.get('http://lucumr.pocoo.org/feed.atom')
feed = atoma.parse_atom_bytes(response.content)
feed.title.value


```

