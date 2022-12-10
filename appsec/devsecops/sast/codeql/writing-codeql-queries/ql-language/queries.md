# Queries

Общий вид:

```
from /* ... variable declarations ... */
where /* ... logical formula ... */
select /* ... expressions ... */
```

Помимо [expressions](https://codeql.github.com/docs/ql-language-reference/expressions/#expressions) можно использовать еще **as, order by ... asc, desc**:

```
from int x, int y
where x = 3 and y in [0 .. 2]
select x, y, x * y as product, "product: " + product
order by y desc
```

## Query predicate

Это возвращающий результат предикат с аннотацией query:

```
query int getProduct(int x, int y) {
  x = 3 and
  y in [0 .. 2] and
  result = x * y
}
```

Он возвращает все множества, которые вычислил предикат — x, y, result:

| x | y | result |
| - | - | ------ |
| 3 | 0 | 0      |
| 3 | 1 | 3      |
| 3 | 2 | 6      |

В чем плюс использовать этот тип предиката — его можно использовать как промежуточное вычисление в других QL запросах:

```
class MultipleOfThree extends int {
  MultipleOfThree() { this = getProduct(_, _) }
}
```
