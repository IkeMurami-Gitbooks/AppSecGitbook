# Генерация wordlists

## Tools

### gitscraper

Тулза анализирует открытые репозитории гита для выявления схожих путей, API, и тп для генерации wordlists, для последующих пентестов, бб [https://github.com/adamtlangley/gitscraper](https://github.com/adamtlangley/gitscraper)

### mentalist

Создание словарей (GUI/python): [https://github.com/sc0tfree/mentalist](https://github.com/sc0tfree/mentalist)

### crunch

инструмент генерации строк для брута \
Пример использования: `$ crunch 5 5 trust >>pass`\
Общий вид: `crunch <минимальная длина> <максимальная длина>` и\
дополнительные опции `>>pass` отвечает за запись полученных комбинаций в файл `pass.txt`

