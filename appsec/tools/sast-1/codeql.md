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

CodeQL Queries: [https://github.com/github/codeql](https://github.com/github/codeql)

## Get Started

Есть два способа работать с CodeQL — [CLI](https://codeql.github.com/docs/codeql-cli/) и [плагин](https://codeql.github.com/docs/codeql-for-visual-studio-code/) для Visual Studio Code

### CodeQL CLI

#### Install

Скачиваем нужный нам архив: [https://github.com/github/codeql-cli-binaries/releases](https://github.com/github/codeql-cli-binaries/releases)

Распаковываем и добавляем в PATH путь до `<extraction-path>/codeql`.

Проверяем, что все работает:

```
Available languages:
$ codeql resolve languages

You can download some “CodeQL packs” containing pre-compiled queries you would like to run.
$ codeql pack download <pack-name> [...pack-name]

Core packs (a good place to start):
- codeql/cpp-queries
- codeql/csharp-queries
- codeql/go-queries
- codeql/java-queries
- codeql/javascript-queries
- codeql/python-queries
- codeql/ruby-queries

Скачать все квери
$ git clone https://github.com/github/codeql codeql-queries
Проверить, какие codeql пакеты загружены (он их находит как-то серчем по всем папкам?)
$ codeql resolve qlpacks

Здесь будут:
codeql/{language}-queries — пакеты кверей для каждого языка, они будут запускаться при каждом анализе
codeql/{language}-all — пакеты библиотек кверей, которые содержат квери (control flow, data flow, ...). Будут полезны для написания правил
codeql/{language}-examples — примеры кверей для тех, кто хочет писать свои квери
legacy packs — пакеты кверей к старым версиям продуктов

```

#### Usage

Прежде, чем начать, надо создать свою CodeQL-базу, которая будет содержать все необходимое для анализа вашего кода. Ее можно создать самому или скачать с Github.

В CodeQL-database [входит](https://codeql.github.com/docs/codeql-overview/codeql-glossary/#codeql-database):

* queryable data, extracted from the code.
* a source reference, for displaying query results directly in the code.
* query results.
* log files generated during database creation, query execution, and other operations.

Github [создает и сохраняет](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#downloading-codeql-databases-from-github-com) CodeQL-databases для огромного количества open-source проектов. Проверить, что база для репозитория есть, можно следующей командой:

```
gh api /repos/<owner>/<repo>/code-scanning/codeql/databases/
```

Скачать ее:

```
gh api /repos/<owner>/<repo>/code-scanning/codeql/databases/<language> -H 'Accept: application/zip' > path/to/local/database.zip
```

О том, как использовать CodeQL CLI в вашей CI, смотри [тут](https://docs.github.com/en/code-security/secure-coding/using-codeql-code-scanning-with-your-existing-ci-system/configuring-codeql-cli-in-your-ci-system). Как сканить код через Github Actions, смотри [тут](https://docs.github.com/en/code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/setting-up-code-scanning-for-a-repository).

Создать свою базу (из корня проекта):

```
codeql database create <database> --language=<language-identifier>
```

Языки:

| Language              | Identifier   |
| --------------------- | ------------ |
| C/C++                 | `cpp`        |
| C#                    | `csharp`     |
| Go                    | `go`         |
| Java                  | `java`       |
| JavaScript/TypeScript | `javascript` |
| Python                | `python`     |
| Ruby                  | `ruby`       |

Про другие ключи: [https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#running-codeql-database-create](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#running-codeql-database-create)

Пример для JS/TS проекта:

```
codeql database create --language=javascript --source-root <folder-to-extract> <output-folder>/javascript-database
```

По умолчанию, файлы в node\_modules и bower\_components директории не будут извлечены.

Для других языков (JS/TS/Python/Ruby), которые не требуют сборки проекта, смотри тут особенности: [https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-non-compiled-languages](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-non-compiled-languages)

Для языков, требующих сборки проекта (C/C++/Go/C#/Java) — [https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-compiled-languages](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-compiled-languages).

### VSCode Plugin

Документация: [https://codeql.github.com/docs/codeql-for-visual-studio-code/](https://codeql.github.com/docs/codeql-for-visual-studio-code/)

#### Install

Ставим расширение CodeQL от Microsoft. У нас должен быть прописан путь до codeql cli.

#### Usage

Создаем CodeQL-базу, подключаем ее в VSCode.

Создаем свою библиотеку правил или запускаем существующие квери.

## About

### Sites

CodeQL Project: [https://codeql.github.com/](https://codeql.github.com/)

Github Repo — официальный репозиторий с CodeQL кверями: [https://github.com/github/codeql](https://github.com/github/codeql)

### Papers

Материал от Swordfish Security: [https://habr.com/ru/company/swordfish\_security/blog/541554/](https://habr.com/ru/company/swordfish\_security/blog/541554/)

Example Usage CodeQL searching bugs into VPN Client: [https://medium.com/csg-govtech/hunting-bugs-in-accel-ppp-with-codeql-8370e297e18f](https://medium.com/csg-govtech/hunting-bugs-in-accel-ppp-with-codeql-8370e297e18f)
