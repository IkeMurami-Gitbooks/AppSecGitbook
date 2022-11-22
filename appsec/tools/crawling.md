---
description: >-
  Процесс автоматищированного получения информации (вроде сорцов, копии сайта и
  тп)
---

# Crawling

### .git crawling

DVCS-Ripper (Perl) - Git web crawling [https://github.com/kost/dvcs-ripper](https://github.com/kost/dvcs-ripper)

```
docker run --rm -it -v /path/to/host/work:/work:rw k0st/alpine-dvcs-ripper rip-git.pl -v -u http://www.example.org/.git
```

## Screen Web Crawling

### Aquatone

git:  [https://github.com/michenriksen/aquatone](https://github.com/michenriksen/aquatone)

Особенности:

* Написан на Go
* Автор говорит, что работает программа  хорошо в связке с: amass, sublist3r, subfinder, fierce, gobuster, knockpy
* Может брать на вход URL/хосты/выход из nmap/masscan

Возможно годится на то, чтобы сканить вебчики и смотреть скрины

Пример использования:&#x20;

```
cat hosts.txt | aquatone -ports 80,443,3000,3001 -out ~/aquatone/example.com
```

Баги: если в -out будут относительные пути — скриншотов не будет!

В папке будут:&#x20;

* скриншоты
* html-страницы/заголовки
* единый репорт, в котором все наглядно
* url'ы всех вебчиков обнаруженных

### gowitness

Библиотека на go для создания скриншотов сайтов [https://github.com/sensepost/gowitness](https://github.com/sensepost/gowitness)

#### Install

```
docker pull leonjza/gowitness
```

#### Usage

Create alias:

for win create gowitness.bat:

```batch
@echo off
call docker run --rm -v %PWD%:/data leonjza/gowitness gowitness %*
```

or use full run docker command:

```
Create alias to command:
on macos: docker run --rm -v $(pwd):/data leonjza/gowitness gowitness
on win (powershell): docker run --rm -v ${pwd}:/data leonjza/gowitness gowitness
```

run

```
gowitness single https://example.com
gowitness file -f /data/urls.list
```

Запустить report server (http://127.0.0.1:7171) по результату (сюда же можно будет докидывать URL ы новые). Не разобрался пока что, как запустить это в batch файле на винде

```
docker run --rm -v ${pwd}:/data -p7171:7171 leonjza/gowitness gowitness report serve --address :7171
```

#### Watching report

Один минус инструмента: если у вас тысячи хостов, результат смотреть через веб нереально. Но есть возможность через небольшую магию python сгруппировать хосты по параметру `perception_hash` из локальной базы `gowitness.sqlite3` из таблицы `urls`. Накидал скрипт для этого (близость хешей не вычислял):

```python
import sqlite3
from pathlib import Path


class Database:
    def __init__(self, path):
        self.path = path

    def get(self, query):
        cursor = self.db.cursor()
        cursor.execute(query)
        result = cursor.fetchall()
        return result

    def connect(self):
        self.db = sqlite3.connect(self.path)
        return self

    def close(self):
        self.db.close()

# Путь до gowitness.sqlite3 базы
GOWITNESS_SQLITE3_PATH ="D:\MyProjects\BugBounty\scope\gowitness\gowitness.sqlite3"
# Путь до директории, куда сохраняем результат
GOWITNESS_ANALYTIC_OUT_DIR = "D:\MyProjects\BugBounty\scope\gowitness\\analytics"
db = Database(GOWITNESS_SQLITE3_PATH).connect()

phash_records = db.get('SELECT DISTINCT perception_hash FROM urls')

for phash_record in phash_records:
    print(phash_record[0])  # 'p:8b49bbe00bcb8f68', calculate from screenshot image
    url_records = db.get(f'SELECT id, final_url, response_code, response_reason FROM urls WHERE perception_hash = "{phash_record[0]}"')
    
    out_file_records = list()
    headers = []
    for url_record in url_records:
        id, final_url, response_code, response_reason = url_record
        out_file_records.append(', '.join([str(id), final_url, str(response_code), response_reason]))

        # Extract headers for that requests
        header_records = db.get(f'SELECT key, value FROM headers WHERE url_id = {id}')
        headers = [f'{header_record[0]}: {header_record[1]}' for header_record in header_records ]
        headers = list(set(headers))  # save unique
        headers.sort()

        # Extract dns names from certificates
        tls_records = db.get('SELECT name ' +  
                             'FROM tls_certificate_dns_names ' + 
                             'WHERE tls_certificate_id IN (' + 
                                'SELECT id AS certificate_id ' + 
                                'FROM tls_certificates ' + 
                                'WHERE tls_id IN (' + 
                                    'SELECT id AS tls_id ' + 
                                    'FROM tls ' + 
                                   f'WHERE url_id = {id}' + 
                                ')' + 
                            ')')
        
        dns_names = [tls_record[0] for tls_record in tls_records]
        dns_names = list(set(dns_names))  # save unique
        dns_names.sort()


    # Save urls with same perception_hash
    phash_value = phash_record[0].split(':')[1]
    Path(GOWITNESS_ANALYTIC_OUT_DIR, phash_value).mkdir()
    with Path(GOWITNESS_ANALYTIC_OUT_DIR, phash_value, 'urls.list').open(mode='w') as out_stream:
        out_stream.write('\n'.join(out_file_records))

    # Save headers
    with Path(GOWITNESS_ANALYTIC_OUT_DIR, phash_value, 'headers.list').open(mode='w') as out_stream:
        headers.sort()
        out_stream.write('\n'.join(headers))

    # Save dns_names
    with Path(GOWITNESS_ANALYTIC_OUT_DIR, phash_value, 'dnsnames.list').open(mode='w') as out_stream:
        dns_names.sort()
        out_stream.write('\n'.join(dns_names))

db.close()
```

## URL crawling from WebApp

### gau

gau (Go) - собирает из открытых источников (из инфы по доменам, из архивов сайтов и тп) по домену - [https://github.com/lc/gau](https://github.com/lc/gau)

### hakrawler

hakrawler (Go): [https://github.com/hakluke/hakrawler](https://github.com/hakluke/hakrawler)

Нормально ставится через go get

Находит меньше, чем через urlgrab, но зато ищет относительные пути в js (правда, через регулярку из linkfinder). Можно использовать в паре с urlgrab

Usage: `hakrawler -url <URL> -depth 3 -linkfinder -js -sitemap -robots -insecure`

### urlgrab

urlgrab (Go) - нормально собирает ссылки со страницы с прогрузкой сайтов [https://github.com/IAmStoxe/urlgrab](https://github.com/IAmStoxe/urlgrab)\
Собирать проект через make build (собирает круто локально в докере)\
Использовать: `urlgrab -url <url> -output-all /directory/result/out -render-js`-ignore-ssl

### gal

gal (Bash): [https://github.com/YashGoti/gal](https://github.com/YashGoti/gal)

## Tokens & API keys crawling

zile (Python) [https://github.com/xyele/zile](https://github.com/xyele/zile)

detect-secrets (Python) [https://github.com/Yelp/detect-secrets](https://github.com/Yelp/detect-secrets)

## JS change monitoring

JSMon (Python) - мониторим изменение скриптов  на сайте. В случае изменений -> получаем оповещение в телеграмм. [https://github.com/robre/jsmon](https://github.com/robre/jsmon)

## Копирование сайтов

goclone (Go) - выкачиваем сайт: [https://github.com/imthaghost/goclone](https://github.com/imthaghost/goclone)

Замечание по проекту: надо поправить и сделать pull request

```go
// https://github.com/imthaghost/goclone

url := "https://example.com"
parser.ValidateURL(url) && parser.ValidateDomain(url)  // сомнительно, конечно: лучше использовать какую-то др библиотеку
file.CreateProject(urlDomain)  // здесь пздц: потенциально directory path traversal -> исправить. Extractor - отвечает за сохранение информации со страницы в локальную директорию
crawler.Crawl(url, projectpath)  // Использует Colly. Не извлекает анонимные скрипты, оставляет их в html
html.LinkRestructure(projectpath)  // подставляет img, script, css в html + форматирует
exec.Commnad("open", "http://localhost:5000").Start()
server.Serve(projectpath)  // не понятно, как этим пользоваться

```



## Analyze webpack bundle (blackbox)

GradeJS

## TODO: опробовать katana

Инструмент от projectdiscovery (Go) — [https://github.com/projectdiscovery/katana](https://github.com/projectdiscovery/katana)
