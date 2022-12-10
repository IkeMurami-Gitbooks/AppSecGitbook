# CodeQL Queries

## Common

CodeQL запрос имеет расширение `.ql` и его общий вид следующий:

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

**from** — определяет те множества, над которыми производится выборка. Эти множества мы можем определять сами (через **class**) и определять над ними функции, например:

```
import javascript

class SmallInt extends int {
    SmallInt() { this in [1..10] }
    int my_func() { result = this * this }
}

from SmallInt x, SmallInt y, SmallInt z
where x.my_func() + y.my_func() = z.my_func()
select x, y, z
```

## Query Metadata

`@kind` — определяет как интерпретировать и отображать результат запроса. Этот параметр обязателен, если хотите, чтобы ваш запрос работал из CodeQL CLI, так же он обязателен для Open Source запросов.&#x20;

* `@kind problem` — alert query metadata — the results as a simple alert
* `@kind path-problem` — path query metadata — документирует алерт как последовательность мест в коде
* `@kind diagnostic` — diagnostic query metadata — to identify the results as troubleshooting data about the extraction process
* Summary query metadata must contain `@kind metric` and `@tags summary` to identify the results as summary metrics for the CodeQL database

Подробнее про метаданные: [https://codeql.github.com/docs/writing-codeql-queries/metadata-for-codeql-queries/](https://codeql.github.com/docs/writing-codeql-queries/metadata-for-codeql-queries/)

## Query Help Files

README для CodeQL queries: [https://codeql.github.com/docs/writing-codeql-queries/query-help-files/](https://codeql.github.com/docs/writing-codeql-queries/query-help-files/)

## Import

Импортировать можем [библиотеки](https://codeql.github.com/docs/ql-language-reference/modules/#library-modules) и [модули](https://codeql.github.com/docs/ql-language-reference/modules/#modules). Стандартные библиотеки под разные языки: [https://codeql.github.com/docs/codeql-language-guides/](https://codeql.github.com/docs/codeql-language-guides/). Эти библиотеки содержат инструмента для построения различных видов анализов для path query, включая `data flow`, `control flow` и `taint-tracking`. Для того, чтобы вычислить `path graph`, в path query следует импортировать `data flow` библиотеки.

## Where statement

```
where t.getAge() > 30
  and (t.getHairColor() = "brown" or t.getHairColor() = "black")
  and not t.getLocation() = "north"
```

Плохая практика в **from** запихивать множества, вывод которых не будет происходить в **select**. В этом случае можно использовать функцию **exists**. Сравни:

Без exists:

```
from Person t, string c
where t.getHairColor() = c
select t
```

С exists:

```
from Person t
where exists(string c | t.getHairColor() = c)
select t
```

exists — пример [explicit quantified formula](https://codeql.github.com/docs/ql-language-reference/formulas/#exists).

## Predicates

A [QL predicate](https://codeql.github.com/docs/ql-language-reference/predicates/#predicates) is a mini-query that expresses a relation between various pieces of data and describes some of their properties.

Ex:

```
predicate isCountry(string country) {
  country = "Germany"
  or
  country = "Belgium"
  or
  country = "France"
}

predicate hasCapital(string country, string capital) {
  country = "Belgium" and capital = "Brussels"
  or
  country = "Germany" and capital = "Berlin"
  or
  country = "France" and capital = "Paris"
}

from Location loc
where isCountry(loc.name)
select loc.name
```

## Function

```
Person relativeOf(Person p) { parentOf*(result) = parentOf*(p) }
```

## Classes

Определение своего класса String через **class**:

```
class StringWrap extends Literal {
    StringWrap() {
        this.toString().charAt(0) = "\"" or this.toString().charAt(0) = "'"
    }
    int length() {
        result = this.toString().length()
    }
}

from StringWrap c
where c.length() > 3
select c
```

Переопределение предиката

```
class Child extends Person {
  /* the characteristic predicate */
  Child() { this.getAge() < 10 }

  /* a member predicate */
  override predicate isAllowedIn(string region) {
    region = this.getLocation()
  }
}
```
