# Creating alert queries

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
