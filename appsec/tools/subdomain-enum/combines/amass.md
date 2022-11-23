# Amass

## Intro

[Amass ](https://github.com/OWASP/Amass)(Go) — проект OWASP.

Состоит из 5 компонентов

* `amass intel` — получить сведения об организации по домену, братские домены
* `amass enum` — перебор и маппинг
* `amass viz` — визуализация
* `amass track` — изменения между сканированиями
* `amass db` — работа с amass graph db (в частности, можно экспортнуть в Maltego)

## Install via Docker

Загружаем образ с docker.hub:

```
docker pull caffix/amass
```

На windows сделал следующий alias:

```batch
@echo off
call docker run -v D:\\Tools\\AppSec\\amass:/.config/amass/ caffix/amass %*
```

## Components

### intel

Братские домены организации:

```
amass intel -d example.com -whois
```

IP to Domain:

```
amass intel -active -addr 93.28.17.161,93.28.21.181,93.28.16.93
```

Информация по организации:

```
amass intel -org 'Example Ltd'
```

Информация по ASN

```
amass intel -active -asn 222222 -ip
```

### enum

Passive enum — из открытых источников

```
amass enum -passive -d owasp.org -src
```

Active enum — tls/certificates, zone transferring, port scanning, bruteforce, ...

```
$ amass enum -active -d owasp.org -brute -w /root/dns_lists/deepmagic.com-top50kprefixes.txt -src -ip -dir amass4owasp -config /root/amass/config.ini -o amass_results_owasp.txt
```

DNS Wordlist: [https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS](https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS)

### viz

Построить d3 граф:

```
amass viz -d3 -dir amass4owasp
```

Так же, можем [экспортировать ](https://github.com/OWASP/Amass/blob/master/doc/user\_guide.md#importing-owasp-amass-results-into-maltego)результат в Maltego:

```
amass viz -maltego
```

### track

Посмотреть изменения в сканировании

```
amass track  -config /root/amass/config.ini -dir amass4owasp -d owasp.org -last 2
```

### db

Посмотреть историю сканирований:

```
amass db -dir amass4owasp -list
```

Посмотреть ассеты, обнаруженные в определенное сканирование

```
amass db -dir amass4owasp -d owasp.org -enum 1 -show
```

## Amass using pipeline

Разработчики Amass предлагают следующий подход для периодического сканирования и поиска поддоменов:

* amass intel: проверяем ASN ids, по ним ищем новые родительские домены и их поддомены
* amass enum: пассивным сканирование мониторим новые поддомены

Если хотим настроить регулярное сканирование, разработчики крайне рекомендуют вложить свое время в настройку config.ini. Пример автоматизации поиска новых ресурсов на Bash от разработчиков Amass:

```bash
1. APP_TOKEN="$1"
2. USER_TOKEN="$2"

3. amass enum -src -active -df ./domains_to_monitor.txt -config ./regular_scan.ini -o ./amass_results.txt -dir ./regular_amass_scan -brute -norecursive
4. RESULT=$(amass track -df ./domains_to_monitor.txt -config ./regular_scan.ini -last 2 -dir ./regular_amass_scan | grep Found | awk '{print $2}')

5. FINAL_RESULT=$(while read -r d; do if grep --quiet "$d" ./all_domains.txt; then continue; else echo "$d"; fi; done <<< $RESULT)

6. if [[ -z "$FINAL_RESULT" ]];
7. FINAL_RESULT="No new subdomains were found"
8. else
9. echo "$FINAL_RESULT" >> ./all_domains.txt
10. fi
11. wget https://api.pushover.net/1/messages.json --post-data="token=$APP_TOKEN&user=$USER_TOKEN&message=$FINAL_RESULT&title=$TITLE" -qO- > /dev/null 2>&1 &
```

На мобильный телефон придет уведомление через pushover.net API.

## Using with Go

Amass можно использовать как библиотеку в Go.&#x20;

Пример Go скрипта — поиск ip по доменам:

```go
package amass

import (
	"fmt"
	"math/rand"
	"time"

	"github.com/OWASP/Amass/v3/config"
	"github.com/OWASP/Amass/v3/datasrcs"
	"github.com/OWASP/Amass/v3/enum"
	"github.com/OWASP/Amass/v3/systems"
)

func AmassTest() {
	// Seed the default pseudo-random number generator
	rand.Seed(time.Now().UTC().UnixNano())

	// Setup the most basic amass configuration
	cfg := config.NewConfig()
	cfg.AddDomain("example.ru")

	sys, err := systems.NewLocalSystem(cfg)
	if err != nil {
		return
	}
	sys.SetDataSources(datasrcs.GetAllSources(sys, false))

	e := enum.NewEnumeration(cfg, sys)
	if e == nil {
		return
	}
	defer e.Close()

	e.Start()
	for _, o := range e.ExtractOutput(nil, false) {
		fmt.Println(o.Name)
	}
}
```

## Docs & Papers

Wiki:&#x20;

* [https://github.com/OWASP/Amass/blob/master/doc/user\_guide.md](https://github.com/OWASP/Amass/blob/master/doc/user\_guide.md)
* [https://github.com/OWASP/Amass/blob/master/doc/tutorial.md](https://github.com/OWASP/Amass/blob/master/doc/tutorial.md)
