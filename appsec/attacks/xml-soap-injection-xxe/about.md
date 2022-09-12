# About

## About

Происходит парсинг XML файлов, полученных из недоверенных источников. При этом сам XML парсер сконфигурирован таким образом (или настроен по умолчанию), что позволяет использовать external entity references и/или XInclude для произвольного доступа к файлам системы. Даже если в результате никакие данные не возвращаются пользователю, существуют различные техники получения данных злоумышленником при наличии неправильно сконфигурированного парсера.

XML DTD [состоит](https://www.w3schools.com/xml/xml\_dtd\_building.asp) из:

* Elements
* Attributes
* Entities
* PCDATA
* CDATA

### Объявление Internal DTD

```xml
<?xml version="1.0"?>
<!DOCTYPE note [
 <!ELEMENT note (to,from,heading,body)>
  <!ELEMENT to (#PCDATA)>
  <!ELEMENT from (#PCDATA)>
  <!ELEMENT heading (#PCDATA)>
  <!ELEMENT body (#PCDATA)>
]>
<note>
 <to>Tove</to>
 <from>Jani</from>
 <heading>Reminder</heading>
 <body>Don't forget me this weekend</body>
</note> 
```

### Объявление External DTD

```markup
<?xml version="1.0"?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note> 
```

note.dtd:

```markup
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to (#PCDATA)>
<!ELEMENT from (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body (#PCDATA)> 
```

### Использование  Internal Entity

<pre class="language-markup"><code class="lang-markup">&#x3C;?xml version="1.0"?>
&#x3C;!DOCTYPE note [
<strong>    &#x3C;!ENTITY % myparameterentity "my parameter entity value" >
</strong>]>
&#x3C;note>
    &#x26;myparameterentity;
&#x3C;/note></code></pre>

## Mitigation

Для избежания XXE следует отключить парсинг любых Document Type Declarations (DTDs) в недоверенных данных.\
Если это невозможно, то следует отключить, по крайней мере, парсинг external general/parameter entities и XInclude.\
Кроме того, для избежания [DOS атак](https://en.wikipedia.org/wiki/Billion\_laughs\_attack) следует установить лимиты на расширения entities.\
Больше про предотвращение XXE в Java можно узнать [здесь](https://cheatsheetseries.owasp.org/cheatsheets/XML\_External\_Entity\_Prevention\_Cheat\_Sheet.html#java).
