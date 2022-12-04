# Writing CodeQL queries

The important types of query are:

* **Alert queries**: queries that highlight issues in specific locations in your code
* **Path queries**: queries that describe the flow of information between a source and a sink in your code

You can add custom queries to [CodeQL packs](https://codeql.github.com/docs/codeql-cli/about-codeql-packs/) to analyze your projects with “[Code scanning](https://docs.github.com/en/code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/about-code-scanning)”, use them to analyze a database with the “[CodeQL CLI](https://codeql.github.com/docs/codeql-cli/#codeql-cli),” or you can contribute to the standard CodeQL queries in our [open source repository on GitHub](https://github.com/github/codeql).

## CodeQL Packs

Note: The CodeQL package management functionality, including CodeQL packs, is currently available as a **beta** release and is subject to change.

CodeQL packs are used to create, share, depend on, and run CodeQL queries and libraries. You can publish your own CodeQL packs and download packs created by others. CodeQL packs contain queries, library files, query suites, and metadata.

There are two types of CodeQL packs: **query** packs and **library** packs.

* Query packs are designed to be run. When a query pack is published, the bundle includes all the transitive dependencies and a compilation cache. This ensures consistent and efficient execution of the queries in the pack.
* Library packs are designed to be used by query packs (or other library packs) and do not contain queries themselves. The libraries are not compiled and there is no compilation cache included when the pack is published.

The standard CodeQL packages for all supported languages are published in the [GitHub Container registry](https://github.com/orgs/codeql/packages). The [CodeQL repository](https://github.com/github/codeql) contains source files for the standard CodeQL packs for all supported languages.

Два основных файла, определяющий пакет, — `qlpack.yml` (отвечает за пакет) и `codeql-pack.lock.yml` (отвечает за тесты). Подробнее про их настройку [тут](https://codeql.github.com/docs/codeql-cli/about-codeql-packs/).

### Creating and working with CodeQL packs via CodeQL CLI

Source: [https://codeql.github.com/docs/codeql-cli/creating-and-working-with-codeql-packs](https://codeql.github.com/docs/codeql-cli/creating-and-working-with-codeql-packs)

Создать пакет (`scope` — имя пользователя github или организации, `pack` — имя пакета):

```
codeql pack init <scope>/<pack>
```

По умолчанию создается query-пакет, если хотим library — правим вручную.

Добавить в зависимость другой пакет:

```
codeql pack add <scope>/<name>@x.x.x <scope>/<other-name>
```

Или можно вручную подправить `qlpack.yml` и сделать `codeql pack install`.

Как опубликовать и использовать ваш CodeQL Pack: [https://codeql.github.com/docs/codeql-cli/publishing-and-using-codeql-packs/](https://codeql.github.com/docs/codeql-cli/publishing-and-using-codeql-packs/)

## CodeQL Queries

CodeQL запрос имеет расширение .ql и его общий вид следующий:

```
/**
 *
 * Query metadata
 *
 */

import /* ... CodeQL libraries or modules ... */

/* ... Optional, define CodeQL classes and predicates ... */

from /* ... variable declarations ... */
where /* ... logical formula ... */
select /* ... expressions ... */
```

### Query Metadata

`@kind` — определяет как интерпретировать и отображать результат запроса. Этот параметр обязателен, если хотите, чтобы ваш запрос работал из CodeQL CLI, так же он обязателен для Open Source запросов.&#x20;

* `@kind problem` — alert query metadata — the results as a simple alert
* `@kind path-problem` — path query metadata — документирует алерт как последовательность мест в коде
* `@kind diagnostic` — diagnostic query metadata — to identify the results as troubleshooting data about the extraction process
* Summary query metadata must contain `@kind metric` and `@tags summary` to identify the results as summary metrics for the CodeQL database

Подробнее про метаданные: [https://codeql.github.com/docs/writing-codeql-queries/metadata-for-codeql-queries/](https://codeql.github.com/docs/writing-codeql-queries/metadata-for-codeql-queries/)

### Query Help Files

README для CodeQL queries: [https://codeql.github.com/docs/writing-codeql-queries/query-help-files/](https://codeql.github.com/docs/writing-codeql-queries/query-help-files/)

### Import

Импортировать можем [библиотеки](https://codeql.github.com/docs/ql-language-reference/modules/#library-modules) и [модули](https://codeql.github.com/docs/ql-language-reference/modules/#modules). Стандартные библиотеки под разные языки: [https://codeql.github.com/docs/codeql-language-guides/](https://codeql.github.com/docs/codeql-language-guides/). Эти библиотеки содержат инструмента для построения различных видов анализов для path query, включая `data flow`, `control flow` и `taint-tracking`. Для того, чтобы вычислить `path graph`, в path query следует импортировать `data flow` библиотеки.

### Defining the results of a query

Изменяя `select` выражение, можем определять, как результат нашего запроса будет отображаться. Если мы хотим использовать наши квери для скана, то их вывод должет соответствовать определенному формату и иметь определенные параметры в метаданных в исходном коде запроса.

Для alert-запросов выражение select имеет следующий вид:

```
select <element>, <string>
```

element — найденный элемент кода, к которому относится алерт

string — наше сообщение к алерту

Но могут быть и доп элементы, в том числе, можем вставлять какие-то строки в сообщение через `$@`:

```
Ex1:

select access, "Variable $@ may be null at this access " + msg + ".", var.getVariable(),
  var.getVariable().getName(), reason, "this"
  
Ex2:

select f, "This case label is a duplicate of $@.", e, e.toString()
```

Пример запроса, который ищет дупликаты кода с помощью общей библиотеки `CodeDuplication.qll` в java:

```
import java
import external.CodeDuplication

from File f, File other, int percent
where similarFiles(f, other, percent)
select f, "This file is similar to another file."
```

## TODO

Off documentation: [https://codeql.github.com/docs/writing-codeql-queries/about-codeql-queries/](https://codeql.github.com/docs/writing-codeql-queries/about-codeql-queries/)\
CTF based on CodeQL (see on main page): [https://codeql.github.com/](https://codeql.github.com/)&#x20;

Build AppSec: [https://github.blog/2022-11-04-how-to-mitigate-owasp-vulnerabilities-while-staying-in-the-flow/](https://github.blog/2022-11-04-how-to-mitigate-owasp-vulnerabilities-while-staying-in-the-flow/)\
[https://resources.github.com/appsec/](https://resources.github.com/appsec/)

Challenges:\
[https://securitylab.github.com/ctf/uboot/](https://securitylab.github.com/ctf/uboot/)

Познакомиться с основными типами данных:`codeql/<lang>/ql/examples/snippets`

Примеры:

* [https://gist.github.com/japroc/5f6a04d3ecda912008c72c9d852fae06](https://gist.github.com/japroc/5f6a04d3ecda912008c72c9d852fae06)
* [https://gist.github.com/japroc/d2dc2c5de180c668c68ae7668388b70c](https://gist.github.com/japroc/d2dc2c5de180c668c68ae7668388b70c)
* [https://gist.github.com/japroc/4c0f664d419b451b93cac8303f9239c5](https://gist.github.com/japroc/4c0f664d419b451b93cac8303f9239c5)
* [https://gist.github.com/japroc/16e86edd0863cc3e2f2365d931937488](https://gist.github.com/japroc/16e86edd0863cc3e2f2365d931937488)

Сообщество в тг: [https://t.me/codeql](https://t.me/codeql)

