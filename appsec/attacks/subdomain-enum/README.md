---
description: Поиск поддоменов
---

# Subdomain Enum

## Tools&#x20;

### Прям машины над источниками

#### OWASP Amass

git: [https://github.com/OWASP/Amass/tree/master/amass/sources](https://github.com/OWASP/Amass/tree/master/amass/sources)

Особенности:

* Написан на Go
* subdomain enum and network mapping - это автоматизация над кучей инструментов и баз других
* Есть поддержка плагинов (Lua)

tutorial: [https://www.dionach.com/blog/how-to-use-owasp-amass-an-extensive-tutorial/](https://www.dionach.com/blog/how-to-use-owasp-amass-an-extensive-tutorial/)

```
Домен по ip
amass intel -active -addr 93.28.17.161,93.28.21.181,93.28.16.93

```

Пример Go скрипта: ip по доменам

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

#### Monitorizer

git: [https://github.com/BitTheByte/Monitorizer/](https://github.com/BitTheByte/Monitorizer/)

Особенности:

* Источники: aiodnsbrute, subfinder, sublist3r, dnsrecon, dnscan, amass
* Интеграция со slack (для уведомлений)
* Интеграция с acunetix
* Написан на python
* Для своей работы требует веб-сервер с linux (например, на Amazon EC2)

#### Findomain

git: [https://github.com/Edu4rdSHL/findomain](https://github.com/Edu4rdSHL/findomain)

Выглядит так, что запускают amass + domain enum. Делают это на постоянке и продают под ключ. \
Бесплатно доступна только их враппер над amass. Все остальное (мониторинг хостов) - за деньги.

Особенности:

* Источники: amass, subfinder, sublist3r, assetfinder
* Интеграция с кучей мессенджеров (для уведомлений)
* За деньги предоставляет услуги по управлению continues scanning: за нас занимаются поднятием инфры, постоянно сканят (на предмет поднятия http приложений, портов, поддоменов и тп) и шлют уведомления
* Написан на Rust
* Поддержа elasticsearch, shodan и тп
* Поддерживает интеграцию с Rapid7 Project Sonar (это сканер сети (типо nessus?))

#### dnsdumbster

Это сайт, вбиваешь домен, он находит куча всего и как бонусом строит карту зависимостей серверов (кто на кого ссылается и тому подобное). Очень круто и есть то, что не находят RiskIQ, shodan.

### Тулзы, покрывающие маленькое число ресурсов

subfinder (Go) - [https://github.com/projectdiscovery/subfinder](https://github.com/projectdiscovery/subfinder)

Sublist3r (Python) - брут + OSINT [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)

TheHarvester (Python) - поиск поддоменов, почт, виртуальных хостов, открытых портов/банеры и др информацию через поисковые движки (OSINT; упор именно на OSINT. amass не дергает, но там столько источников, что надо сравнивать). [https://github.com/laramies/theHarvester](https://github.com/laramies/theHarvester)\
Включает в себя sublist3r и поддержку многих других источников (например, Hunter.io, который не поддерживается amass). Можно использовать вместо Sublist3r в других связках.

waybackurls (Go) src: VirusTotal, Archive.org (Wayback Machine), CommonCrawl - [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls)

AssetFinder (Go)  - еще один инструмент. Тоже OSINT. В TODO стоит интеграция с другими ресурсами интересными (например, RiskIQ). В общем, надо анализировтаь и сравнивать источники с другими инструментами.   [https://github.com/tomnomnom/assetfinder](https://github.com/tomnomnom/assetfinder)\
Пример использования: `assetfinder -subs-only my.domain.com | httprobe`

### Брут по спискам

subEnum (python) - [https://github.com/itsKindred/subEnum](https://github.com/itsKindred/subEnum)

KnockPy (support VirusTotal) [https://github.com/guelfoweb/knock](https://github.com/guelfoweb/knock)

shuffleDNS (Go)- обертка над massdns. Вроде как очень быстрый брут [https://github.com/projectdiscovery/shuffledns](https://github.com/projectdiscovery/shuffledns)

#### massdns

massdns - очень быстрое сканирование поддоменов\
&#x20;бывают ошибки на поддоменах 4+ уровня. если надо прям точно, поставь меньше конкурентных запросов и таймаут + число попыток увеличить на ошибки\
Если надо прям точно, можно использовать минимум DNS-резолвов, прям самые популярные (единицы, восьмерки и т.д.), но это не хорошо, тк близко к DDoS.\
Если VPS слабая/канал узок - особо не тюню, в штатке через пайп и вывод в файл.\
Но обычно по классике: по процессу на ядро, конкурентно оставляю 10к запросов (пробовал больше на 4х процессах, но теряться начинало много), по сокету на процесс с сохранением в файл, минимум вывода -oS\
Флаги:\
\--processes 4\
\-s 10000 (кол-во конкурентных запросов это по дефолту, при нескольких процесах я не трогаю этот флаг, при одном можно повысить)\
\-oS (минимум вывод, только запись, как в зоне хранится)\
\-socket-count 1 (кол-во сокетов на процесс, по дефолту 1, я это не трогаю)\
\-i 500-1000(милисек, время между резолвами одного домена, дефолт 500, но можно повысить, субъективно повышало точность, но вот скорость падает заметно)\
\-c 50 (сколько раз пробовать отправить запрос для домена, дефолтно 50, можно повысить, но смысл спорный)

Поделюсь своими флагами, может быть интересно будет\
\--verify-ip -r resolvers7 -i 2000 -c 50 -o Sin

### Источники

crt.sh/?q=\<query> - поиск по сертам зарегистрированные домены

SecurityTrails.com - охеренный сайт для поиска поддоменов и IP
