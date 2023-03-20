# Getting Started

## Install

```
python3 -m pip install semgrep
```

## Usage

Пример запуска:

```
semgrep \
   --config "p/golang" \
   -l go \
   -f /path/to/custom/rules1 \
   -f /path/to/custom/rules2 \
   [/path/to/src]
```

Через `--config` указываем ruleset (можно несколько).&#x20;

Через `-l` можно ограничить язык(и).

Через `-f` пути до наших правил кастомных.
