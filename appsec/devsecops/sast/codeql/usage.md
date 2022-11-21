# Usage

## Create CodeQL-database

Прежде, чем начать, надо создать свою CodeQL-базу, которая будет содержать все необходимое для анализа вашего кода. Ее можно создать самому или скачать с Github.

В CodeQL-database [входит](https://codeql.github.com/docs/codeql-overview/codeql-glossary/#codeql-database):

* queryable data, extracted from the code.
* a source reference, for displaying query results directly in the code.
* query results.
* log files generated during database creation, query execution, and other operations.

### Download existing

Github [создает и сохраняет](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#downloading-codeql-databases-from-github-com) CodeQL-databases для огромного количества open-source проектов. Проверить, что база для репозитория есть, можно следующей командой:

```
gh api /repos/<owner>/<repo>/code-scanning/codeql/databases/
```

Скачать ее:

```
gh api /repos/<owner>/<repo>/code-scanning/codeql/databases/<language> -H 'Accept: application/zip' > path/to/local/database.zip
```

### CodeQL CLI in your CI

О том, как использовать CodeQL CLI в вашей CI, смотри [тут](https://docs.github.com/en/code-security/secure-coding/using-codeql-code-scanning-with-your-existing-ci-system/configuring-codeql-cli-in-your-ci-system). Как сканить код через Github Actions, смотри [тут](https://docs.github.com/en/code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/setting-up-code-scanning-for-a-repository).

### Create your CodeQL-database

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

#### Creating database for non-compiled languages

Для других языков (JS/TS/Python/Ruby), которые не требуют сборки проекта, смотри тут особенности: [https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-non-compiled-languages](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-non-compiled-languages)

#### Creating database for compiled languages

Для языков, требующих сборки проекта (C/C++/Go/C#/Java) — [https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-compiled-languages](https://codeql.github.com/docs/codeql-cli/creating-codeql-databases/#creating-databases-for-compiled-languages).

## Run queries

Подключаем созданную базу в VSCode

Открывает репозиторий кверей как воркспейс для VSCode

Выбираем кверю и пкм — Run query on Multiple Databases. ![](../../../../.gitbook/assets/изображение.png)

Можно запустить над отдельным файлом.
