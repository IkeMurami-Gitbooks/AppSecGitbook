# Creating path queries

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

### Creating path queries

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
