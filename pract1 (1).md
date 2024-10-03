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
./reg banner
```

В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.

## Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.

## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).

## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.

## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 

## Полезные ссылки

Линукс в браузере: https://bellard.org/jslinux/

ShellCheck: https://www.shellcheck.net/

Разработка CLI-приложений

Общие сведения

https://ru.wikipedia.org/wiki/Интерфейс_командной_строки
https://nullprogram.com/blog/2020/08/01/
https://habr.com/ru/post/150950/

Стандарты

https://www.gnu.org/prep/standards/standards.html#Command_002dLine-Interfaces
https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html
https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html

Реализация разбора опций

Питон

https://docs.python.org/3/library/argparse.html#module-argparse
https://click.palletsprojects.com/en/7.x/
