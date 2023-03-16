# Javascript

## Библиотеки

### codeql/javascript-all

Базовая библиотека для построения кверей над javascript (javascript, json, html).

```
codeql pack add codeql/javascript-all
```

### codeql/javascript-queries

Библиотека запросов — `codeql/javascript-queries` ([source](https://github.com/github/codeql/tree/codeql-cli/latest/javascript/ql/src)). Содержит в себе следующие CodeQL Suites:

* `code-scanning`: queries run by default in CodeQL code scanning on GitHub
* `security-extended`: queries from `code-scanning`, plus extra security queries with slightly lower precision and severity.
* `security-and-quality`: queries from `code-scanning`, `security-extended`, plus extra maintainability and reliability queries.

Примеры кверей из этого пакета: [https://codeql.github.com/codeql-query-help/javascript/](https://codeql.github.com/codeql-query-help/javascript/)

Пробовал ее установить как зависимость, получил ошибку при установке:

```
$ codeql pack install
A fatal error occurred: Package version failure. No compatible version found: codeql/suite-helpers@>=0.4.4 <=0.4.4.
```

