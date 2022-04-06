# Description

Атака заключается в том, что атакующий отсылает на сервер запрос, который провоцирует сервер на еще один запрос ко внутренним или внешним ресурсам, и соотв их возвращает атакующему\
Пример:\
[http://server/site?url=http://127.0.0.1/secret\_file](http://server/site?url=http://127.0.0.1/secret\_file)

## Tricks

### SSRF in FFmpeg HLS & ImageMagick

Есть какая-то известная история с SSRF в случае, если сть возможность лить видео и изображения и эти файлы далее препроцессятся. link (так же смотри линки внутри): [https://hackerone.com/reports/1062888](https://hackerone.com/reports/1062888)

## Impact

* Scanning of internal network.
* Read internally hosted files/data.
* Access services listening on the loopback interface (127.0.0.1)
* Read local system files.
* If it’s hosted on AWS, access the AWS REST interface

Надо не забывать, что IP адрес может быть представлен разными формами, например в виде инта: [https://www.browserling.com/tools/ip-to-dec](https://www.browserling.com/tools/ip-to-dec)
