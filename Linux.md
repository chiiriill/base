# Linux CLI command


## Раскрытие скобок



## grep

grep (**g**lobally search for a **re**gular expression and **p**rint matching
lines) &mdash; одна из самых частых команд, которая используется в shell scripting.

Основное предназначение &mdash; это построковый поиск по регулярному выражению в файле:

```
grep
Matches patterns in input text.
 - Search for a pattern within a file:
   grep {{search_pattern}} {{path/to/file}}
```

```console
$ grep "ro\{2\}t" /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

В регулярных выражениях поддерживаются стандартные `., *, +, ?, {n,m}, \w, \s, [:alpha:]` и т.д.

* `.` означает любой символ
* `*` означает matching нуля или более элементов, например, `.*` &mdash; это произвольное количество символов (возможно пустое), а `a*` &mdash; произвольное количество букв `a`
* `+` означает один или более символов; `[0-9]+` означает хотя бы одна цифра из диапазона `0-9`.
* `{n,m}`, `{n}` &mdash; количество повторений; `(aba){3}` матчит 3 раза строку `aba`, а `(aba){3,5}` от 3 до 5 раз, а `(aba){,5}` не более 5 раз.
* `?` &mdash; 0 или 1 группа; `https?://` матчит `http://` и `https://`, а `(https)?://` матчит `https://` и `://`.
* `\w` &mdash; любой словесный символ (word symbol), `\s` &mdash; любой пробельный символ (пробел, новая строка и т.д.).
* `[]` &mdash; группы, например, `[a-z]` матчит одну маленькую букву, `[a-z_]` матчит одну маленькую букву или `_`, `[0-3]{4}` матчит 4 раза цифры от 0 до 3. Отрезки, которые поддерживаются, &mdash; это латинские буквы (маленькие и большие, цифры). Можно сделать отрицание, поставив `^` в начало, например, `[^a-z&]` матчит всё, кроме маленьких латинских букв и символа `&`.

Регулярные выражения отличаются своей семантикой иногда, но выше предоставлены те, которые поддерживаются везде. Я советую синтаксис [RE2](https://github.com/google/re2/wiki/Syntax). Он лучше из-за того, что разрешает только те операции, по которым поиск будет идти полиномиальное время.

grep может выводить строки файлов с опцией `-n`, имена файлов с помощью `-H`(
бывает полезно для поиска и быстрой замены). А также может рекурсивно искать в
папке во всех файлах с помощью опции `-r`.

grep очень удобен для pipe поиска, например, достаточно часто используется вот так:

```console
$ cmd | grep $search_pattern
```

Можно не учитывать регистр с опцией `-i` и инвертировать поиск с помощью
`-v`, а показать контекст на &pm;N строк &mdash; `-C N`. Остальные опции можете
почитать в man, я указал на самые часто используемые.

Я стал для кода больше использовать [ripgrep](https://github.com/BurntSushi/ripgrep),
потому что он лучше и быстрее ищет по коду, минуя всякие .git директории и
бинарные файлы по умолчанию.

Вот кстати [пример](https://stackoverflow.com/questions/201323/how-can-i-validate-an-email-address-using-a-regular-expression) страшного рег. выражения валидации емейла.

# find

Одна из самых насыщенных утилит для поиска файлов в директориях. Примеры скажут
сами за себя:

```bash
# Find all directories named src
$ find . -name src -type d

# Find all python files that have a folder named test in their path
$ find . -path '*/test/*.py' -type f

# Find all files modified in the last day
$ find . -mtime -1

# Find all zip files with size in range 500k to 10M
$ find . -size +500k -size -10M -name '*.tar.gz'
```

Можно `find`-у после `-exec` передавать команды для исполнения над найденными файлами:

```console
# Delete all files with .tmp extension
$ find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
$ find . -name '*.png' -exec convert {} {}.jpg \;
```

Часто используется команда xargs, которая умеет передавать stdout
программы как аргументы другой, например:

```console
$ find . -name '*.tmp' | xargs rm
```

Сделает тоже самое, более универсально, но менее оптимально.

## curl

curl является отличным инструментом для не очень серьёзного скрейпинга каких-то
сайтов, а также дебага проблем с браузерами.

```
- Download the contents of an URL to a file:
   curl {{http://example.com}} -o {{filename}}

 - Download a file, saving the output under the filename indicated by the URL:
   curl -O {{http://example.com/filename}}

 - Download a file, following [L]ocation redirects, and automatically [C]ontinuing (resuming) a previous file transfer:
   curl -O -L -C - {{http://example.com/filename}}

 - Send form-encoded data (POST request of type application/x-www-form-urlencoded). Use -d @file_name or -d @'-' to read from STDIN:
   curl -d {{'name=bob'}} {{http://example.com/form}}

 - Send a request with an extra header, using a custom HTTP method:
   curl -H {{'X-My-Header: 123'}} -X {{PUT}} {{http://example.com}}
```

Часто включают опцию `--silent`, чтобы зря не забивать stderr. Для полных
HTTP запросов ещё используют `-K` опцию для чтения из файла.

В браузерах по F12 в разделе Network можно скопировать запросы как curl запросы,
это стало стандартом.

## sed

sed (**s**tream **ed**itor) &mdash; это утилита для запуска скриптов, которые как-то меняют
файлы, однако используется в большинстве своём построчными заменами одного
регулярного выражения на другие:

```console
# Замена и вывод в stdout
$ sed 's/expr_1/expr_2/' file.txt
# Inplace замена
$ sed -i 's/expr_1/expr_2/' file.txt
```

В `expr_1` можно ставить скобки, а в `expr_2` можно использовать их в порядке как
\1, например:

```console
$ cat file.txt
some_thing1
some_thing2
some_thing3
some_thing4
some_thing5
some_thing6
some_thing7
another_string

$ sed 's/some_\(thing[0-9]\)/\1/' file.txt
thing1
thing2
thing3
thing4
thing5
thing6
thing7
another_string

$ sed -E 's/some_(thing[0-9])/\1/' file.txt
thing1
thing2
thing3
thing4
thing5
thing6
thing7
another_string
```

В целом, у `sed` аргумент принимает скрипт. Если он начинается с `s`, то идёт
поиск по всем строкам; если есть числа перед `s`, например, `4,17s`, то поиск
идёт с 4 до 17 строки; если строка `/apple/s` то операция произведётся только
со всеми, где есть `apple`, `!s` &mdash; отрицание, например:

```console
$ sed -E '1,3!s/some_(thing[0-9])/\1/' file.txt
some_thing1
some_thing2
some_thing3
thing4
thing5
thing6
thing7
kek
```

В целом, `s` &mdash; просто одна команда, за которой идут аргументы. Есть много других
команд, например, `d` &mdash; delete, `y` &mdash; транcлитерация, `i` &mdash; вставка перед
текстом:

```console
$ seq 10 | sed '1,3d'
4
5
6
7
8
9
10
$ seq 10 | sed '1~4!d' # 1 с шагом 4
1
5
9
$ echo "hello world" | sed 'y/abcdefghij/0123456789/'
74llo worl3
```

То есть структура такая: сначала выбор строк (по номерам или по
регулярному выражению), потом однобуквенная команда (возможно с отрицанием
предыдущего условия), потом её аргументы.

Как пример, sed чрезвычайно полезен в фильтрации тестовых данных и исправлении
каких-то опечаток.

## awk

Используйте Python. Забудьте про эту команду.(￢_￢)