# Another tools

## Тулзы, покрывающие маленькое число ресурсов

subfinder (Go) - [https://github.com/projectdiscovery/subfinder](https://github.com/projectdiscovery/subfinder)

Sublist3r (Python) - брут + OSINT [https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)

TheHarvester (Python) - поиск поддоменов, почт, виртуальных хостов, открытых портов/банеры и др информацию через поисковые движки (OSINT; упор именно на OSINT. amass не дергает, но там столько источников, что надо сравнивать). [https://github.com/laramies/theHarvester](https://github.com/laramies/theHarvester)\
Включает в себя sublist3r и поддержку многих других источников (например, Hunter.io, который не поддерживается amass). Можно использовать вместо Sublist3r в других связках.

waybackurls (Go) src: VirusTotal, Archive.org (Wayback Machine), CommonCrawl - [https://github.com/tomnomnom/waybackurls](https://github.com/tomnomnom/waybackurls)

AssetFinder (Go)  - еще один инструмент. Тоже OSINT. В TODO стоит интеграция с другими ресурсами интересными (например, RiskIQ). В общем, надо анализировтаь и сравнивать источники с другими инструментами.   [https://github.com/tomnomnom/assetfinder](https://github.com/tomnomnom/assetfinder)\
Пример использования: `assetfinder -subs-only my.domain.com | httprobe`
