# CodeQL Queries

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
