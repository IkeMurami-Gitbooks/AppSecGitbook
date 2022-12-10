# Types

## Primitives

Primitive types — boolean, float, int, string, date

## Classes

Определение своих классов:

```
class SmallInt extends int {
  SmallInt() { this = [1 .. 10] }
}

class OneTwoThree extends int {
  SmallInt divisor;   // declaration of the field `divisor`

  OneTwoThree() { // characteristic predicate
    this = 1 or this = 2 or this = 3
  }

  string getAString() { // member predicate
    result = "One, two or three: " + this.toString()
  }

  predicate isEven() { // member predicate
    this = 2
  }
}
```

Супертип класса может быть указан через **extends** или **instanceof**.

Можно использовать предикат из конкретного класса:

```
1.(OneTwoThree).getAString()
```
