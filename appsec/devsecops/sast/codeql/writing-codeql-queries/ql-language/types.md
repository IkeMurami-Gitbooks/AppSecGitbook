# Types

## Primitives

Primitive types — boolean, float, int, string, date

## Classes

### Concrete class

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

### Abstract class

```
abstract class SqlExpr extends Expr {
  ...
}
```

### Override predicates

```
class OneTwo extends OneTwoThree {
  OneTwo() {
    this = 1 or this = 2
  }

  override string getAString() {
    result = "One or two: " + this.toString()
  }
}
```

### Multiple inheritance

```
class Two extends OneTwo, TwoThree {}
```

### Non-extending subtypes

Мы можем вызвать метод другого класса через **super**.

```
class Foo extends int {
  Foo() { this in [1 .. 10] }

  string fooMethod() { result = "foo" }
}

class Bar instanceof Foo {
  string toString() { result = super.fooMethod() }
}
```

Переопределить предикат такого типа нельзя (я про Foo).
