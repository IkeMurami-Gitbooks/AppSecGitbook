# About

## About

Происходит парсинг XML файлов, полученных из недоверенных источников. При этом сам XML парсер сконфигурирован таким образом (или настроен по умолчанию), что позволяет использовать external entity references и/или XInclude для произвольного доступа к файлам системы. Даже если в результате никакие данные не возвращаются пользователю, существуют различные техники получения данных злоумышленником при наличии неправильно сконфигурированного парсера.

## Mitigation

Для избежания XXE следует отключить парсинг любых Document Type Declarations (DTDs) в недоверенных данных.\
Если это невозможно, то следует отключить, по крайней мере, парсинг external general/parameter entities и XInclude.\
Кроме того, для избежания [DOS атак](https://en.wikipedia.org/wiki/Billion\_laughs\_attack) следует установить лимиты на расширения entities.\
Больше про предотвращение XXE в Java можно узнать [здесь](https://cheatsheetseries.owasp.org/cheatsheets/XML\_External\_Entity\_Prevention\_Cheat\_Sheet.html#java).
