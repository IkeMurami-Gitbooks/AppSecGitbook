# Predicates

A [QL predicate](https://codeql.github.com/docs/ql-language-reference/predicates/#predicates) is a mini-query that expresses a relation between various pieces of data and describes some of their properties. Also predicates are various conditions that must be satisfied by the results.

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

## Predicates with and without result

Предикаты могут возвращать результат-множество (как функция) или не возвращать (а точнее логическое выражение содержать).

Без результата:

```
predicate isSmall(int i) {
  i in [1 .. 9]
}
```

С результатом:

```
int getSuccessor(int i) {
  result = i + 1 and
  i in [1 .. 9]
}
```

## Reverse and recursive predicates

Это какая-то необычная концепция

### Reverse predicates

Предикат может использовать result не только для возвращения результата, но и для вычисления над самим результатом

```
Person getAChildOf(Person p) {
  p = getAParentOf(result)
}
```

Предикат `getAChildOf` выше вернет такой person, для которого `getAParentOf` вернет **p**.

Это возможно, потому что по-умолчанию, **result** равен входному множеству `Person p`.

### Recursive predicates

Пусть следующий предикат:

```
string getANeighbor(string country) {
  country = "France" and result = "Belgium"
  or
  country = "France" and result = "Germany"
  or
  country = "Germany" and result = "Austria"
  or
  country = "Germany" and result = "Belgium"
}
```

В нем все хорошо, только не сохраняется свойство симметричности (то есть, если Франция соседствует с Германией, то и Германия должна соседствовать с Францией). В частности, эту проблему можно решить с помощью рекурсивного предиката:

```
string getANeighbor(string country) {
  country = "France" and result = "Belgium"
  or
  country = "France" and result = "Germany"
  or
  country = "Germany" and result = "Austria"
  or
  country = "Germany" and result = "Belgium"
  or
  country = getANeighbor(result)
}
```

## Kinds of predicates

Есть три типа предикатов — **non-member predicates**, **member predicates** и **characteristic predicates**. Первый определяется за пределами класса, два других внутри класса (о них в соотв разделе):

```
int getSuccessor(int i) {  // 1. Non-member predicate
  result = i + 1 and
  i in [1 .. 9]
}

class FavoriteNumbers extends int {
  FavoriteNumbers() {  // 2. Characteristic predicate
    this = 1 or
    this = 4 or
    this = 9
  }

  string getName() {   // 3. Member predicate for the class `FavoriteNumbers`
    this = 1 and result = "one"
    or
    this = 4 and result = "four"
    or
    this = 9 and result = "nine"
  }
}
```

## Вычисление предиката над бесконечными множествами

По умолчанию, компилятор не позволит вычислять что-либо над бесконечным множеством. Результат должен быть конечным и вычисляться конечное время. Например, тут будет ругаться:

```
/*
  Compilation errors:
  ERROR: "i" is not bound to a value.
  ERROR: "result" is not bound to a value.
  ERROR: expression "i * 4" is not bound to a value.
*/
int multiplyBy4(int i) {
  result = i * 4
}

/*
  Compilation errors:
  ERROR: "str" is not bound to a value.
  ERROR: expression "str.length()" is not bound to a value.
*/
predicate shortString(string str) {
  str.length() < 10
}
```

Однако, мы можем сказать компилятору, что входные данные точно будет конечным подмножеством, а следовательно и результат будет конечным:

```
bindingset[x] bindingset[y]
predicate plusOne(int x, int y) {
  x + 1 = y
}

from int x, int y
where y = 42 and plusOne(x, y)
select x, y
```

Есть какие-то особенности с bound множествами. Но суть, что для предиката, который должен вернуть результат-множество, запись будет следующей:

```
bindingset[str, len]
string truncate(string str, int len) {
  if str.length() > len
  then result = str.prefix(len)
  else result = str
}

select truncate("hello world", 5)
```
