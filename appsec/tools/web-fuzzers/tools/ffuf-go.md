# ffuf (Go)

## About

веб фаззер на go. [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

## Basic Usage

ffuf -w wordlist -u https://url/FUZZ -c -v

&#x20;\-c - color\
&#x20;\-v - verbose\
&#x20;\-r - allow redirect\
&#x20;\-o out.json\
&#x20;\-replay-proxy http://127.0.0.1:8888/

Не проверял, но возможно проксируются только успешные запросы (после фильтров)

ffuf -w wordlists/SecLists/Discovery/Web-Content/raft-medium-directories-lowercase.txt -u [http://\<domain>/FUZZ](http://37.18.9.109/FUZZ) -c -r -mc 200

## Перебор директорий

```
ffuf -w .../tools/pentest/wordlists/dirsearch/db/dicc.txt -o ffuf_result.json -r -D -u https://example.com/FUZZ -e js,php,asp,aspx,jsp > out.txt
```

Разбор результатов

```python
# Ищем директории
# ffuf -w domain.web.list:TARGET -w D:\\MyProjects\\BugBounty\\tools\\dirsearch\\db\\dicc.txt:DIRECTORY -o ffuf_result.json -r -D -u https://TARGET/DIRECTORY -e php,asp,aspx,jsp
from pathlib import Path
import json


def introspect(obj):
    for key in obj:
        print(f'{key}: {type(obj[key])}')


with Path('D:\\MyProjects\\BugBounty\\scope\\ffuf_result.json').open(mode='r') as inp_stream:
    data = inp_stream.read()
    ffuf_json_result = json.loads(data)
    print(f'commandline: {ffuf_json_result["commandline"]}')
    print(f'time: {ffuf_json_result["time"]}')
    
    # introspect(ffuf_json_result)

    result = dict()

    for ffuf_record in ffuf_json_result['results']:

        TARGET = ffuf_record['input']['TARGET']
        DIRECTORY = ffuf_record['input']['DIRECTORY']
        status      = ffuf_record['status']
        length      = ffuf_record['length']
        words       = ffuf_record['words']
        lines       = ffuf_record['lines']
        duration    = ffuf_record['duration']

        status = str(status)
        length = str(length)
        words = str(words)
        lines = str(lines)
        duration = str(duration)

        if TARGET not in result:
            result[TARGET] = list()

        result[TARGET].append(', '.join([DIRECTORY, status, length, words, lines, duration]))
        # introspect(ffuf_record)
    
    directory = Path('D:\\MyProjects\\BugBounty\\scope\\ffuf_subdirs')
    directory.mkdir(exist_ok=True)
    
    for TARGET in result:
        with Path(directory, TARGET).open(mode='w') as out_stream:
            out_stream.write('\n'.join(result[TARGET]))
        
```

## Virtual Hosts

&#x20;см в соотв разделе

## Перенаправить на Burp

```
-x http://127.0.0.1:8080/
```
