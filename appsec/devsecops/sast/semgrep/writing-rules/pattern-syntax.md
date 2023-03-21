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

