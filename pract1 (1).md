# Практическое занятие №1. Введение, основы работы в командной строке

А.Е. Беляев, РТУ МИРЭА

Научиться выполнять простые действия с файлами и каталогами в Linux из командной строки. Сравнить работу в командной строке Windows и Linux.

## Задача 1

Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).
```
grep '.*' /etc/passwd | cut -d: -f1 | sort
```
![конфа1](https://github.com/user-attachments/assets/b8a7237e-a104-4dea-94f5-899d49701ab8)


## Задача 2

Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:
```
awk '{print $2, $1}' /etc/protocols | sort -nr | head -n 5
```
![конфа 2](https://github.com/user-attachments/assets/a1b0cb63-6878-4664-99d5-068115551efe)


## Задача 3

Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):

```
#!/bin/bash

text=$*
length=${#text}

for i in $(seq 1 $((length + 2))); do
    line+="-"
done

echo "+${line}+"
echo "| ${text} |"
echo "+${line}+"
```
Далее переходим обратно в эмулятор и пишем:
```
dos2unix 3rd.sh
sudo chmod 755 3rd.sh
./ 3rd.sh "hello"
```
![конфа 3](https://github.com/user-attachments/assets/ffb90ef5-d848-41aa-b001-8fc179fd1a65)
## Задача 4

Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

Пример для hello.c:

```
#!/bin/bash

file="$1"

id=$(grep -o -E '\b[a-zA-Z]*\b' "$file" | sort -u)
_________________________________

grep -oE '\b[a-zA-Z_][a-zA-Z0-9_]*\b' hello.c | grep -vE '\b(int|void|return|if|else|for|while|include|stdio)\b' | sort | uniq
```
![конфа 4](https://github.com/user-attachments/assets/9e26ff97-82cb-4df9-a8be-2e57ae619876)

## Задача 5

Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).

Например, пусть программа называется reg:

```
#!/bin/bash
if [ -z "$1" ]; then
    echo "Использование: $0 <имя_команды>"
    exit 1
fi
COMMAND_NAME=$1
if ! command -v "$COMMAND_NAME" &> /dev/null; then
    echo "Команда '$COMMAND_NAME' не найдена."
    exit 1
fi
sudo cp "$(command -v "$COMMAND_NAME")" /usr/local/bin/
sudo chmod 755 /usr/local/bin/"$COMMAND_NAME"
if [ $? -eq 0 ]; then
    echo "Команда '$COMMAND_NAME' успешно зарегистрирована."
else
    echo "Ошибка при регистрации команды '$COMMAND_NAME'."
    exit 1
fi
```

В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.

## Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
```
#!/bin/bash
if [ -z "$1" ]; then
    echo "Использование: $0 <каталог>"
    exit 1
fi
DIR=$1
find "$DIR" -type f \( -name "*.c" -o -name "*.js" -o -name "*.py" \) | while read -r file; do
    first_line=$(head -n 1 "$file")
    if [[ "$file" == *.py && "$first_line" =~ ^# ]]; then
        echo "Файл $file: содержит комментарий в первой строке."
    elif [[ "$file" == *.c && ("$first_line" =~ ^// || "$first_line" =~ ^/\*) ]]; then
        echo "Файл $file: содержит комментарий в первой строке."
    elif [[ "$file" == *.js && "$first_line" =~ ^// ]]; then
        echo "Файл $file: содержит комментарий в первой строке."
    else
        echo "Файл $file: не содержит комментарий в первой строке."
    fi
done
```

## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).
```
#!/bin/bash
if [ -z "$1" ]; then
    echo "Использование: $0 <каталог>"
    exit 1
fi
DIR=$1
declare -A file_hashes
find "$DIR" -type f | while read -r file; do
    file_hash=$(sha256sum "$file" | awk '{print $1}')
    if [[ -n "${file_hashes[$file_hash]}" ]]; then
        echo "Найден дубликат: $file"
        echo "Оригинал: ${file_hashes[$file_hash]}"
    else
        file_hashes["$file_hash"]=$file
    fi
done
```

## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.
```
#!/bin/bash
if [ "$#" -ne 2 ]; then
    echo "Использование: $0 <каталог> <расширение>"
    exit 1
fi
DIR=$1
EXTENSION=$2
ARCHIVE_NAME="archive_$(date +%Y%m%d_%H%M%S).tar"
find "$DIR" -type f -name "*.$EXTENSION" | tar -cvf "$ARCHIVE_NAME" -T -
if [ $? -eq 0 ]; then
    echo "Файлы с расширением .$EXTENSION успешно архивированы в $ARCHIVE_NAME"
else
    echo "Ошибка при создании архива"
    exit 1
fi
```

## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
```
#!/bin/bash
if [ "$#" -ne 2 ]; then
    echo "Использование: $0 <входной_файл> <выходной_файл>"
    exit 1
fi
INPUT_FILE=$1
OUTPUT_FILE=$2
if [ ! -f "$INPUT_FILE" ]; then
    echo "Ошибка: входной файл '$INPUT_FILE' не существует."
    exit 1
fi
sed 's/    /\t/g' "$INPUT_FILE" > "$OUTPUT_FILE"
if [ $? -eq 0 ]; then
    echo "Замена успешно выполнена. Результат сохранён в '$OUTPUT_FILE'."
else
    echo "Ошибка при обработке файла."
    exit 1
fi
```

## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 
```
#!/bin/bash
if [ "$#" -ne 1 ]; then
    echo "Использование: $0 <директория>"
    exit 1
fi
DIR=$1
if [ ! -d "$DIR" ]; then
    echo "Ошибка: Директория '$DIR' не существует."
    exit 1
fi
find "$DIR" -type f -name "*.txt" -empty
if [ $? -eq 0 ]; then
    echo "Поиск завершён."
else
    echo "Ошибка при поиске."
    exit 1
fi

```
