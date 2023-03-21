# Pattern syntax

## Ellipsis operator

`...` — последовательность 0 или более элементов (arguments, statements, parameters, fields, characters).

### Function calls

```
some_function(...)
some_function(..., "someparam", ...)
```

Мы можем искать функции по полному имени или по краткому, Semgrep поймет:

```
django.utils.safestring.mark_safe(...)
mark_safe(...)
```

Любая функция (например, в JS):

```
...($X) { ... }
```

## Metavariables

метапеременные можно использовать для вывода в сообщениях:

```yaml
rules:
  - id: metavariable-message-example
    pattern: $MODEL.set_password(...)
    message: Setting a password on $MODEL
    languages:
      - python
    severity: WARNING
```

## Imports

Semgrep не видит aliases, но заменяет их типами, например:

```
import requests as reqs
```

Semgrep не сможет построить правило над reqs, но построит его над requests и под капотом reqs == requests для Semgrep.

## Deep expression operator

```
<... [your_pattern] ...>
```

Это выражение, вложенное внутрь другого выражения на любую глубину.

Пример с if:

```
pattern: |
  if <... $USER.is_admin() ...>:
    ...
```

The deep expression operator works in:

* `if` statements: `if <... $X ...>:`
* nested calls: `sql.query(<... $X ...>)`
* operands of a binary expression: `"..." + <... $X ...>`
* any other expression context
