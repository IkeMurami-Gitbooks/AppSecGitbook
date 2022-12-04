# CodeQL Packs

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
