# Типы запросов

### Чем опасен TRACE/TRACK метод

С помощью него можно обходить флаг HttpOnly у кук: делаем запрос на хост с поддержкой TRACE/TRACK метода и из тела ответа забираем нашу куку (кука улетит, так как SameSite, все дела).

### WebDAV

Есть такое расширение протокола HTTP — WebDav. Добавляет в HTTP новые типы запросов:

* PROPFIND — получение свойств объекта на сервере в формате [XML](https://ru.wikipedia.org/wiki/XML). Также можно получать структуру [репозитория](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%BF%D0%BE%D0%B7%D0%B8%D1%82%D0%BE%D1%80%D0%B8%D0%B9) (дерево каталогов);
* PROPPATCH — изменение свойств за одну транзакцию;
* MKCOL — создать коллекцию объектов (каталог в случае доступа к файлам);
* COPY — копирование из одного [URI](https://ru.wikipedia.org/wiki/URI) в другой;
* MOVE — перемещение из одного [URI](https://ru.wikipedia.org/wiki/URI) в другой;
* LOCK — поставить блокировку на объекте. WebDAV поддерживает эксклюзивные и общие (shared) блокировки;
* UNLOCK — снять блокировку с ресурса.

Узнать, что сервер его поддерживает можно по HTTP OPTIONS и по заголовку MS-Author-Via.

#### HTTP метод PROPFIND

Пример получения полей:

```markup
PROPFIND /test-dav/ HTTP/1.1
Content-Type: application/xml
Cookie: l=en
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip,deflate
User-Agent: Test WebDav
Host: example.com
Connection: Keep-alive

<?xml version="1.0" encoding="utf-8" ?> 
  <propfind xmlns="DAV:"> 
    <propname/> 
  </propfind>
```

Ссылка: [https://greenbytes.de/tech/webdav/rfc4918.html#n-example---using-\_propname\_-to-retrieve-all-property-names](https://greenbytes.de/tech/webdav/rfc4918.html#n-example---using-\_propname\_-to-retrieve-all-property-names)

#### Замечение

Пусть эндпоинт для webdav: https://example.com/DavWWW/, с какой-то вероятностью файлы будут доступны на smb шаре: \\\localhost\DavWWW\somefile

### JSONP

Это протокол, применяемый в вебе.  TODO

В этом протоколе есть параметр callback, который указывает какую функцию вызвать при обработке ответа.

```
GET /jsonp?callback=some-func
```
