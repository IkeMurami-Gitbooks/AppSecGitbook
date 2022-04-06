# wfuzz

## About

Web App Fuzzing - фаззинг параметров в запросах на веб приложение\
miniManual\
[https://wfuzz.readthedocs.io/en/latest/](https://wfuzz.readthedocs.io/en/latest/)

## Bug: pycurl

![](<../../../../.gitbook/assets/изображение (7).png>)

Решение см в разделе установки pycurl под macos: [https://wfuzz.readthedocs.io/en/latest/user/installation.html](https://wfuzz.readthedocs.io/en/latest/user/installation.html)

## Help

Хелпа: wfuzz -h\
Пример использования:

```
wfuzz -c -w /path/to/wordlist/list -b "MyCookie=Value" -d "firstPostParameter=FUZZ&secon=test" --hs "if be -> not show result" http://192.1.23.34:4545/test.htm

где:
-с - вывод разноцветный
-w - путь до списка для фаззинга
-b - указать куки
-d - для перебора параметров при POST запросе

-f file.txt - out to file
-f file.txt,json - out to file in json format
-L - допускать редиректы

--hs - если будет строка в ответе, то результат выводиться не будет (для отсеивания левых результатов)
--hc - фильтр по статусу ответа
Последним параметром URL
```

\*В Kali Linux словари лежат в /usr/share/wordlists\
Пример словаря для фаззинга с разной тематикой (логины, пароли и т.п.) - SecLists

Если используешь wfuzz, то он принимает строки из stdin:

```
python3 -c "print(['../'i for i in range(10)], sep='\n')" | wfuzz -z stdin "http://target/FUZZ"
```

Запустить по нескольким хостам (по сути использование нескольких словарей)

```
-u HFUZZ/WFUZZ -w <path_to_urls>:HFUZZ -w <path_to_wordlist>:WFUZZ
```

## Пример через python script

```python
import wfuzz

TARGET = "https://example.com/FUZZ"
EXTENSIONS = ["aspx", "asp", "asmx", "html", "js"]
PAYLOAD_FILE_NAME = "wordlists/dirsearch/db/dicc.txt"

payloads = []
with open(PAYLOAD_FILE_NAME) as payload_stream:
    line = payload_stream.readline()[:-1]
    while line:
        if "%EXT%" in line:
            line = payload_stream.readline()[:-1]  # if need with extensions -> delete this row
            continue
            for ext in EXTENSIONS:
                payloads.append(line.replace('%EXT%', ext))
        else:
            payloads.append(line)
        line = payload_stream.readline()[:-1]

payloads = wfuzz.get_payload(payloads)


for r in payloads.fuzz(url=TARGET, hc=[404]):
    print(r)

```
