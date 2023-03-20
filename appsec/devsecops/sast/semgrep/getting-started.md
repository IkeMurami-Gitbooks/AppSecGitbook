# Getting Started

## Install

```
brew install semgrep
python3 -m pip install semgrep
docker run --rm -v "${PWD}:/src" returntocorp/semgrep semgrep --config=auto
```

## VSCode

У Semgrep есть [расширения](https://semgrep.dev/docs/extensions/#editor) для IDE, в частности, для VSCode — [semgrep-vscode](https://marketplace.visualstudio.com/items?itemName=semgrep.semgrep).

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

## Ignore

Если нам нужно игнорировать какие-то директории, то мы можем

* Через комментарий в коде

```
// nosemgrep
// nosemgrep: rule-id
# nosemgrep
```

* Через файл [.semgrepignore](https://raw.githubusercontent.com/returntocorp/semgrep/develop/cli/src/semgrep/templates/.semgrepignore) в директории. [Подробнее](https://semgrep.dev/docs/ignoring-files-folders-code/#defining-files-and-folders-in-semgrepignore) про синтаксис.
* Semgrep может опираться на ваш файл `.gitignore`, если он есть.
* По умолчанию, Semgrep не смотрит в `/test`, `/tests`, `/vendors`. Если вам надо их включить в анализ, добавьте .semgrepignore без указания этих директорий.
* Semgrep не станет смотреть по умолчанию (это поведение может быть изменено для больших файлов и файлов с неизвестным расширением, но не для двоичных файлов):
  * Большие файлы (больше 1MB)
  * Двоичные файлы
  * Файлы, с расширением не из белого списка

Подробнее про исключения: [https://semgrep.dev/docs/ignoring-files-folders-code/#customizing-ignore-behavior](https://semgrep.dev/docs/ignoring-files-folders-code/#customizing-ignore-behavior)



