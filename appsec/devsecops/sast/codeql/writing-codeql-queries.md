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

## Creating alert queries

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

### Как указать место коде

Документация: [https://codeql.github.com/docs/writing-codeql-queries/providing-locations-in-codeql-queries/](https://codeql.github.com/docs/writing-codeql-queries/providing-locations-in-codeql-queries/)

\+ Есть предикат `.toString()`.

### About data flow analysis

Documentation (здесь есть ссылки на примеры для разных языков): [https://codeql.github.com/docs/writing-codeql-queries/about-data-flow-analysis/](https://codeql.github.com/docs/writing-codeql-queries/about-data-flow-analysis/)

Data flow analysis is used to compute the possible values that a variable can hold at various points in a program, determining how those values propagate through the program and where they are used. Data flow analysis is used extensively in **path queries**. To learn more about path queries, see “[Creating path queries](https://codeql.github.com/docs/writing-codeql-queries/creating-path-queries/).”

The data flow graph is computed using [classes](https://codeql.github.com/docs/ql-language-reference/types/#classes) to model the program elements that represent the graph’s nodes. The flow of data between the nodes is modeled using [predicates](https://codeql.github.com/docs/ql-language-reference/predicates/#predicates) to compute the graph’s edges.

Computing an accurate and complete data flow graph presents several challenges:

* It isn’t possible to compute data flow through standard library functions, where the source code is unavailable.
* Some behavior isn’t determined until run time, which means that the data flow library must take extra steps to find potential call targets.
* Aliasing between variables can result in a single write changing the value that multiple pointers point to.
* The data flow graph can be very large and slow to compute.

To overcome these potential problems, two kinds of data flow are modeled in the libraries:

* **Local data flow**, concerning the data flow within a single function. When reasoning about local data flow, you only consider edges between data flow nodes belonging to the same function. It is generally sufficiently fast, efficient and precise for many queries, and it is usually possible to compute the local data flow for all functions in a CodeQL database.
* **Global data flow**, effectively considers the data flow within an entire program, by calculating data flow between functions and through object properties. Computing global data flow is typically more time and energy intensive than local data flow, therefore queries should be refined to look for more specific sources and sinks.

### Normal data flow vs taint tracking

В общем, есть разница между двумя этими подходами. **Normal data flow** будет отслеживать только `x` в следующем выражении:

```
y = x + 1
```

А **taint tracking** подход начнет отслеживать и `y`. В QL этот подход расширяет **normal data flow** анализ в **taint tracking library** через **предикаты**, которые отслеживают, если значение будет передано между узлами кода. &#x20;

## Creating path queries[¶](https://codeql.github.com/docs/writing-codeql-queries/creating-path-queries/#creating-path-queries)

You can create path queries to visualize the flow of information through a codebase.

Path queries written with CodeQL are particularly useful for analyzing data flow as they can be used to track the path taken by a variable from its possible starting points (`source`) to its possible end points (`sink`). To model paths, your query must provide information about the `source` and the `sink`, as well as the data flow steps that link them.

### Path query template

```
/**
 * ...
 * @kind path-problem
 * ...
 */

import <language>
// For some languages (Java/C++/Python) you need to explicitly import the data flow library, such as
// import semmle.code.java.dataflow.DataFlow
import DataFlow::PathGraph
...

from MyConfiguration config, DataFlow::PathNode source, DataFlow::PathNode sink
where config.hasFlowPath(source, sink)
select sink.getNode(), source, sink, "<message>"
```

Where:

* `DataFlow::Pathgraph` is the path graph module you need to import from the standard CodeQL libraries.
* `source` and `sink` are nodes on the [path graph](https://en.wikipedia.org/wiki/Path\_graph), and `DataFlow::PathNode` is their type.
* `MyConfiguration` is a class containing the predicates which define how data may flow between the `source` and the `sink`.

The following sections describe the main requirements for a valid path query.

### Generating path explanations

In order to generate path explanations, your query needs to compute a [path graph](https://en.wikipedia.org/wiki/Path\_graph). To do this you need to define a [query predicate](https://codeql.github.com/docs/ql-language-reference/queries/#query-predicates) called `edges` in your query. This predicate defines the edge relations of the graph you are computing, and it is used to compute the paths related to each result that your query generates. You can import a predefined `edges` predicate from a path graph module in one of the standard data flow libraries. In addition to the path graph module, the data flow libraries contain the other `classes`, `predicates`, and `modules` that are commonly used in data flow analysis.

```
import DataFlow::PathGraph
```

This statement imports the `PathGraph` module from the data flow library (`DataFlow.qll`), in which `edges` is defined.

You can also import libraries specifically designed to implement data flow analysis in various common frameworks and environments, and many additional libraries are included with CodeQL. To see examples of the different libraries used in data flow analysis, see the links to the built-in queries above or browse the [standard libraries](https://codeql.github.com/codeql-standard-libraries).

For all languages, you can also optionally define a `nodes` query predicate, which specifies the nodes of the path graph that you are interested in. If `nodes` is defined, only edges with endpoints defined by these nodes are selected. If `nodes` is not defined, you select all possible endpoints of `edges`.

#### **Defining your own `edges` predicate**

You can also define your own `edges` predicate in the body of your query. It should take the following form:

```
query predicate edges(PathNode a, PathNode b) {
/** Logical conditions which hold if `(a,b)` is an edge in the data flow graph */
}
```

For more examples of how to define an `edges` predicate, visit the [standard CodeQL libraries](https://codeql.github.com/codeql-standard-libraries) and search for `edges`.

### Declaring sources and sinks

You must provide information about the `source` and `sink` in your path query. These are objects that correspond to the nodes of the paths that you are exploring. The name and the type of the `source` and the `sink` must be declared in the `from` statement of the query, and the types must be compatible with the nodes of the graph computed by the `edges` predicate.

If you are querying C/C++, C#, Go, Java, JavaScript, Python, or Ruby code (and you have used `import DataFlow::PathGraph` in your query), the definitions of the `source` and `sink` are accessed via the `Configuration` class in the data flow library. You should declare all three of these objects in the `from` statement. For example:

```
from Configuration config, DataFlow::PathNode source, DataFlow::PathNode sink
```

The configuration class is accessed by importing the data flow library. This class contains the predicates which define how data flow is treated in the query:

* `isSource()` defines where data may flow from.
* `isSink()` defines where data may flow to.

### Defining flow conditions

The `where` clause defines the logical conditions to apply to the variables declared in the `from` clause to generate your results. This clause can use [aggregations](https://codeql.github.com/docs/ql-language-reference/expressions/#aggregations), [predicates](https://codeql.github.com/docs/ql-language-reference/predicates/#predicates), and logical [formulas](https://codeql.github.com/docs/ql-language-reference/formulas/#formulas) to limit the variables of interest to a smaller set which meet the defined conditions.

When writing a path queries, you would typically include a predicate that holds only if data flows from the `source` to the `sink`.

You can use the `hasFlowPath` predicate to specify flow from the `source` to the `sink` for a given `Configuration`:

```
where config.hasFlowPath(source, sink)
```

### Select clause

Select clauses for path queries consist of four ‘columns’, with the following structure:

```
select element, source, sink, string
```

### Debugging data-flow queries using partial flow

If a data-flow query doesn’t produce the results you expect to see, you can use partial flow to debug the problem. In CodeQL, you can use [data flow analysis](https://codeql.github.com/docs/writing-codeql-queries/about-data-flow-analysis/#about-data-flow-analysis) to compute the possible values that a variable can hold at various points in a program. A typical data-flow query looks like this:

```
class MyConfig extends TaintTracking::Configuration {
  MyConfig() { this = "MyConfig" }

  override predicate isSource(DataFlow::Node node) { node instanceof MySource }

  override predicate isSink(DataFlow::Node node) { node instanceof MySink }
}

from MyConfig config, DataFlow::PathNode source, DataFlow::PathNode sink
where config.hasFlowPath(source, sink)
select sink.getNode(), source, sink, "Sink is reached from $@.", source.getNode(), "here"
```

The same query can be slightly simplified by rewriting it without [path explanations](https://codeql.github.com/docs/writing-codeql-queries/creating-path-queries/#creating-path-queries):

```
from MyConfig config, DataFlow::Node source, DataFlow::Node sink
where config.hasPath(source, sink)
select sink, "Sink is reached from $@.", source.getNode(), "here"
```

If a data-flow query that you have written doesn’t produce the results you expect it to, there may be a problem with your query. You can try to debug the potential problem by following the steps described below: [https://codeql.github.com/docs/writing-codeql-queries/debugging-data-flow-queries-using-partial-flow/](https://codeql.github.com/docs/writing-codeql-queries/debugging-data-flow-queries-using-partial-flow/)
