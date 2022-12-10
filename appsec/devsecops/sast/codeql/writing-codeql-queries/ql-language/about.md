# About

Source: [https://codeql.github.com/docs/ql-language-reference/about-the-ql-language/](https://codeql.github.com/docs/ql-language-reference/about-the-ql-language/)

QL is a declarative, object-oriented query language.

Here are a few prominent conceptual and functional differences between general purpose programming languages and QL:

* QL does not have any imperative features such as assignments to variables or file system operations.
* QL operates on sets of tuples and a query can be viewed as a complex sequence of set operations that defines the result of the query.
* QL’s set-based semantics makes it very natural to process collections of values without having to worry about efficiently storing, indexing and traversing them.
* In object oriented programming languages, instantiating a class involves creating an object by allocating physical memory to hold the state of that instance of the class. In QL, classes are just logical properties describing sets of already existing values.
