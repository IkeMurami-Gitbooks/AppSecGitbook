# Get Started

Есть два способа работать с CodeQL — [CLI](https://codeql.github.com/docs/codeql-cli/) и [плагин](https://codeql.github.com/docs/codeql-for-visual-studio-code/) для Visual Studio Code.

## CodeQL CLI

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

## VSCode Plugin

Документация: [https://codeql.github.com/docs/codeql-for-visual-studio-code/](https://codeql.github.com/docs/codeql-for-visual-studio-code/)

Ставим расширение CodeQL от Microsoft. У нас должен быть прописан путь до codeql cli в PATH.
