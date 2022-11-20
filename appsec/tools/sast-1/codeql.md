# CodeQL

## Preview

CodeQL + Semgrep — вроде как куда лучше Checkmarx, тк:

* Код открыт
* Правила активно пополняются сообществом (в том числе через h1 и github)
* Активно поддерживается и используется Github для скана репозиториев

Есть плагины для vscode для работы с этими SAST'ами.

На Github в фильтре по языкам можно вбить `CodeQL` и найти примеры скриптов для анализатора.

Раньше был проект SemmleQL: [https://semmle.com/](https://semmle.com/) отдельный, сейчас он стал частью Github. LGTM стал корпоративным продуктом (его [документация](https://help.semmle.com/home/help/home.html)).

## Documentations

CodeQL Project: [https://codeql.github.com/](https://codeql.github.com/)

Документация: [https://codeql.github.com/docs/](https://codeql.github.com/docs/)

## Get Started

Есть два способа работать с CodeQL — [CLI](https://codeql.github.com/docs/codeql-cli/) и [плагин](https://codeql.github.com/docs/codeql-for-visual-studio-code/) для Visual Studio Code

### CodeQL CLI

Скачиваем нужный нам архив: [https://github.com/github/codeql-cli-binaries/releases](https://github.com/github/codeql-cli-binaries/releases)

Распаковываем и добавляем в PATH путь до `<extraction-path>/codeql`.

Проверяем, что все работает:

```
Available languages:
$ codeql resolve languages
```

## About

### Sites

CodeQL Project: [https://codeql.github.com/](https://codeql.github.com/)

Github Repo — официальный репозиторий с CodeQL кверями: [https://github.com/github/codeql](https://github.com/github/codeql)

### Papers

Материал от Swordfish Security: [https://habr.com/ru/company/swordfish\_security/blog/541554/](https://habr.com/ru/company/swordfish\_security/blog/541554/)

Example Usage CodeQL searching bugs into VPN Client: [https://medium.com/csg-govtech/hunting-bugs-in-accel-ppp-with-codeql-8370e297e18f](https://medium.com/csg-govtech/hunting-bugs-in-accel-ppp-with-codeql-8370e297e18f)
