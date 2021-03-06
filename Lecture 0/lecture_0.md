# Лекция 0

Темы:

- Обзор курса;
- Инструменты разработки на С;
- Первая программа на C.

## Краткий обзор курса

Данный курс из 4 лекций предназначен для базового освоения синтаксиса 
языка программирования C. В ходе этого курса вам предлагается ознакомиться 
с основынми инструментами и приемами разработки программ на языке C. 
Полученных знаний хватит для того, чтобы автоматизировать несложные процессы, описывать алгоритмы на языке C, а также освоить другие языки 
программирования с более сложными концепциями: C++, C#, Java, Python и др.

## Инструменты разработчика

В первую очередь для разработки на языке С понадобятся следующие инструменты:

- Текстовый редактор;
- Компилятор;
- Отладчик;
- Система контроля версий.

Некоторые из инструментов, о которых речь идет ниже, являются проектами с открытым исходным кодом (open source), другие распространяются бесплатно в составе различных программ для студентов или имеют бесплатный план использования. Так или иначе я вам рекомендую получить студенческие пакеты от Github и JetBrains:

- [Github Student Developer Pack](https://education.github.com/pack)
- [JetBrains For Students](https://www.jetbrains.com/student/)

### Текстовый редактор

Существует огромное количество различных текстовых редакторов, среди которых выделяются [Emacs](https://www.gnu.org/software/emacs/) и [Vim](https://www.vim.org/). Это **крайне** мощные инструменты, обладающие большим потенциалом для расширения. Так или иначе, на освоение в них вам потребуется некоторое время ([смотри вот эту ветку](https://stackoverflow.com/questions/11828270/how-to-exit-the-vim-editor)). Среди редакторов попроще зачастую выбирают [Sublime](https://www.sublimetext.com/), [Atom](https://atom.io/) или [Visual Studio](https://code.visualstudio.com/). Несмотря на достаточно низкий уровень вхождения, эта группа редакторов также позволяет использовать (и писать свои) расширения, настраивать работу и внешний вид и т.д.

### Компилятор

Пока что будем рассматривать компилятор как некотурую сущность, котороя переводит наши программы с языка C в машинный код (на самом деле процесс сложнее, но пока детали опустим). В ходе данного курса в качестве компилятора будет использоваться [GCC](https://gcc.gnu.org/). Если вы используете Linux или macOS, то, скорее всего, пакет gcc у вас уже установлен (чтобы убедиться в этом файле введите в вашей командной оболочке `gcc --version`). Если вы используете ОС семейства Windows, то есть как минимум 3 способа для его установки:

- [MinGW](http://mingw.org/)
- [Cygwin](https://cygwin.com/)
- Windows Subststem for Linux

#### MinGW

MinGW, сокращение от "Minimalist GNU for Windows", предоставляет вам порты программ проекта GNU/Linux для использования их в командной оболочке Windows. Более подробно процесс установки MinGW описан [здесь](http://mingw.org/wiki/Getting_Started).

#### Cygwin

Cygwin предоставляет достаточно большую коллекциб программ GNU, а также содержит в себе эмулятор командной оболочки bash. Более подробно процесс установки Cygwin описан [здесь](https://cygwin.com/cygwin-ug-net/ov-ex-win.html).

#### Windows Subststem for Linux

Windows Subststem for Linux - специальный инструмент Windows 10, который позволяет быстро и просто получить доступ к эмулятору bash на Windows. Более подробно процесс установки Windows Subststem for Linux описан [здесь](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

#### Другие варианты

Если ничего из перечисленного выше не близко вашей тонкой душе, то можно проявить фантазию:

- Установить один из множества дистрибутивов Linux в качестве Dual Boot'a на свой компьютер;
- поставить виртуальную машину на свою систему с Linux;
- найти еще один компьютер и поставить на него Linux;
- купить VPS и доступаться к нему по SSH чтобы компилировать свои программы.

И так далее.

#### Компиляция программ на С

Для компиляции будем использовать следующую команду:

```
gcc -ansi -Wall <файл.c> -o <исполняемый файл>
```

Где gcc - программа-компилятор, -ansi - указание стандарта ANSI C, -Wall - указание на вывод всех предупреждений, -o - указание на сбор объектного кода в исполняемый файл. При помощи такой команды мы переведем программу на языке C, которая содержится в <файл.c> в <исполняемый файл>, который можно будет запустить командой `./<исполняемый файл>`. Для отладки будем в процессе компиляции добавлять опцию -g, как в:

```
gcc -ansi -Wall -g <файл.c> -o <исполняемый файл>
```

### Отладчик

В качестве отладчика можно использовать [GDB](https://www.gnu.org/software/gdb/). Сначала компилируем нашу программу с опцией -g, а затем запускаем gdb, передав в качестве параметра исполняемый файл. Для установки точки останова используется команда break. Для запуска программы используется команда run. Для пошагового исполнения программы используется команда start. Команды step и next используются для перехода к следующей инструкции. Step - переход к следующей команде, next - переход к следующей строке кода.

### Система контроля версий

Система контроля версий необходима не только для обеспечения процесса совместной работы нескольких людей над одним проектом. Она также позволяет индивидуальному разработчику эффективно управлять процессом написания исходного кода, разрабатывать новые features, при этом имея возможность откатиться и т.д. В рамках данного курса мы будем использовать [Git](https://git-scm.com/) для контроля версий.

###### Краткая справа по Git

|Команда    |Значение    |
| -         |:----------:|
|git init   |инициализирует git-репозиторий в текущей директории|
|git add    |<путь к файлу или директории> добавляет файлы в staging|
|git commit -m "ваше сообщение"|создает коммит с вашем сообщением|
|git branch <название ветки>|создает новую ветку|
|git checkout <название ветки>|переключается на ветку|
|git checkout --.|отменает все изменения, которые не были добавлены в коммит|
|git checkout <commit hash>|откатиться к коммиту с указаным хешем|

Более подробно Git рассмотрен в других лекциях сообщества Lambda:

- [Лекция 1](https://github.com/lambdafrela/lambda-help/tree/master/lectures/git/lecture_1)
- [Лекция 2](https://github.com/lambdafrela/lambda-help/tree/master/lectures/git/lecture_2)

### IDE

IDE - интегрированные среды разработки объединяют в себе все указанные выше инструменты. Среди доступных инструментов советую обратить внимание на [Clion](https://www.jetbrains.com/clion/) и [Visual Studio](https://www.visualstudio.com/). Последним из них я не пользовался, но игнорировать его популярность тяжело, так что в качестве эксперимента можно рассмотреть и его.

## Первая программа на C

Изучение любого языка по традиции начинается с программы "Hello, World!". Она позволяет попробовать на практике базовый синтаксис языка и узнать о функциях вывода.

Для вывода будем использовать стандартную функцию *printf()*, описание которой содержится в заговочном файле stdio.h:

```C
/* комментарии в С пишутся вот так */

#include <stdio.h> /* директива #include говорит препроцессору, что необходимо включить текст файла stdio.h наш файл */

int main() /* точка вхождения в программу */
{
  printf("Hello, World!\n"); /* Строку заключаем в двойные кавычки */
}
```

Символ *\n* — т.н. управляющая последовательности (escape-sequence — последовательность нескольких символов, обозначающих специальную инструкцию устройства ввода/вывода), которая говорит, что после ее считывания продолжить печать нужно с новой строки. Вот список всех ескейп последовательностей:

|Последовательность| Значение|
| - |:-------------:|
|\a | подача звукового сигнала|
|\b | возврат назад и затирание|
|\f | прогон страницы|
|\n | конец строки|
|\r | возврат каретки|
|\t | горизонтальная табуляция|
|\v | вертикальная табуляция|
|\o00| восьмеричное число|
|\x00| шестнадцатеричное число|
|\\\\ | обратная косая черта|
|\\? | вопросительный знак|
|\\' | одинарная ковычка|
|\\" | двойная кавычка|

**Замечание:** последние четыре последовательности это так называемые *экранированные* символы. Т.е. мы ставим обратный слеш дважды, чтобы показать, что намерены использовать именно этот символ, а не какую-то специальную последовательность, начинающуюся с него, как бы *экранируя* его.

**Замечание 2:** В ОС семейства Windows для перевода строки используется последовательность символов **\r\n**, в то время как в Linux — просто **\n**.

В конце каждой инструкции *языка* C ставится ";". Обратите внимание, что после директивы препроцессора ";" не стоит.
После компиляции и запуска программы мы получаем следующее:

Hello, World!

Voila!

### Форматный вывод при помощи printf()

Конечно, "Hello, World!" — не слишком-то полезная программа. Неплохо было бы что-нибудь подсчитать и вывести результат. Для вывода различных значений при помощи функции printf используются спецификаторы формата.

Для ясности рассмотрим общий вид функции printf:

```C
int printf(const char *format, arg-list);
/* const char *format - строка символов, которую нужно вывести на экран. Также содержит спецификаторы формата
arg-list - список аргументов (переменные, константы), которые нуждаются в выводе */
```

На месте спецификаторов появляются значения аргументов из списка arg-list.

Количество аргументов должно точно соответствовать количеству спецификаторов формата, причем следовать они должны в одинаковом порядке. Например, следующий вызов функции printf():

```C
printf ("Hi %с %d %s", 'с', 10, "there!");
```

Приведет к выводу «Hi с 10 there!».
Строка символов, выводимая на экран, отделяется от аргументов запятой. Аргументы друг от друга также отделяются запятой.

###### Таблица команд форматирования функции printf

|Код| Формат |
| - |:-------------:|
|%с | Символ типа char|
|%d | Десятичное число целого типа со знаком||
|%i | Десятичное число целого типа со знаком|
|%е | Научная нотация (е нижнего регистра)|
|%Е | Научная нотация (Е верхнего регистра)|
|%f | Десятичное число с плавающей точкой|
|%g | Использует код %е или %f — тот из них, который короче (при использовании %g используется е нижнего регистра)|
|%G | Использует код %Е или %f — тот из них, который короче (при использовании %G используется Е верхнего регистра)|
|%о | Восьмеричное целое число без знака|
|%s | Строка символов|
|%u | Десятичное число целого типа без знака|
|%х | Шестнадцатиричное целое число без знака (буквы нижнего регистра)|
|%Х | Шестнадцатиричное целое число без знака (буквы верхнего регистра)|
|%р | Выводит на экран значение указателя|
|%n | Ассоциированный аргумент — это указатель на переменную целого типа, в которую помещено количество символов, записанных на данный момент|
|%% | Выводит символ %|

Кроме того, могут быть применнеы модификаторы типов, например:
%ld — long int,
%hu — short unsigned,
%lf — double.

### Домашнее задание

1. Создать аккаунт на [Github](https://github.com/).
2. Активировать [Github Student Developer Pack](https://education.github.com/pack).
3. Активировать [JetBrains For Students](https://www.jetbrains.com/student/).
4. [Присоединиться](https://vk.com/away.php?to=https%3A%2F%2Flambdainvite.herokuapp.com%2F&cc_key=) к нашему [чату в Slack](https://lambdafrela.slack.com/).
5. Написать программу, которая выводит фразу "Hello, world!".
6. Создать свой git-репозиторий.
7. Загрузить git-репозиторий с программой на Github.
8. Отправить ссылку на него мне на почту s.guliaev@lambda-it.ru.
9. Почитать про GCC [здесь](http://www.network-theory.co.uk/docs/gccintro/).

Связаться со мной всегда можно по указанной выше почти или в чатике в Slack.