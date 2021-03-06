# Task12: Протокол разделения секрета

Протокол разделения секрета основан на интерполяционных многочленах Лагранжа и предоставляет выбор количества ключей
и сколько из них нужно чтобы вскрыть секрет, если хотите увеличить чью-то долю, просто выдайте ему
дополнительный файл секрета, секрет представляет собой тройку (x, v, prime), где x - переменная многочлена f,
такая что f(x) = v (mod prime) 

## Описание протокола

    1) Определчется количество человек K для получения секрета M и количество частей P
    2) Генерируется многочлен f(x)=M + x*a1 + x^2*a2...x^n*aN mod prime, где N=K-1, aI - случайны
    3) Генерируются части секрета f(1)=....=V1 f(2)=....=V2 и т.д. до VP
    4) Части секрета раздаются секрет=(x, vX, prime)
    5) Для генерации секрета решаем уравнения
          f(1)=M + a1   + a2     + ... + aN     = v1 (mod prime)
          f(2)=M + a1*2 + a2*2^2 + ... + aN*2^N = v2 (mod prime)
          ........................................................
          f(Y)=M + a1*Y + a2*Y^2 + ... + aN*Y^N = v1 (mod prime)
      И так взяв любые N=K-1, можно найти все коэфициенты правильно

## Использование

Пользователю предлагается выбрать количество секретов и количество человек для доступа к секрету.
Также пользователь задаёт секрет. И при предоставлении нужного количества секретов программа
может раскрыть секрет или понять что людей не хватает ( Хотя при нехватке людей возможен случай раскрытия,
но непонятно чего, точно не секрета а случайного решения системы )

Файлы секрета помещаются в папку с программой в виде secret-*.seq.
!!!ОСТОРОЖНО возможно случайное удаление прошлого секрета.

P.S. Протокол может быть использован человеком создавшим супербомбу с кодом активации, он разделит секрет(код) на части,
раздаст его наиболее правильным людям, странам и будет наблюдать за их взаимоействием с целью получить контроль над секретом :D 

### Help программы:

usage: main.py [-h] [-restore RESTORE [RESTORE ...]] [-access ACCESS]
               [-parts PARTS]
               [secret [secret ...]]

Программа реализации разделения секрета с помощью многочленов Лагранжа

positional arguments:
  secret                Секрет

optional arguments:
  -h, --help            show this help message and exit
  -restore RESTORE [RESTORE ...]
                        Путь к файлам с разделёнными секретами
  -access ACCESS        Количество человек для доступа к секрету
  -parts PARTS          Количество разделений секрета


## Примеры работы

Для удобства есть сценарий:

execute.bat
--------------------------------------------------------------------------------
@ECHO OFF
python main.py 100
PAUSE
--------------------------------------------------------------------------------

### Логи Программы:

store.file
python main.py -access 4 -parts 10 Super secret message!
Разбиваем секрет на числовые блоки
Для каждого блока создаём набор разделений
Записан файл secret-1.seq c частью секрета
Записан файл secret-2.seq c частью секрета
Записан файл secret-3.seq c частью секрета
Записан файл secret-4.seq c частью секрета
Записан файл secret-5.seq c частью секрета
Записан файл secret-6.seq c частью секрета
Записан файл secret-7.seq c частью секрета
Записан файл secret-8.seq c частью секрета
Записан файл secret-9.seq c частью секрета
Записан файл secret-10.seq c частью секрета

restore.file
python main.py -restore secret-1.seq secret-10.seq secret-5.seq secret-7.seq secret-9.seq
Предоставлено 5 секретных частей
Secret: Super secret message!

badrestore.file
python main.py -restore secret-1.seq secret-10.seq secret-5.seq
Предоставлено 3 секретных частей
Traceback (most recent call last):
  File "main.py", line 146, in <module>
    main(parser.parse_args())
  File "main.py", line 95, in main
    restore(args.restore)
  File "main.py", line 88, in restore
    raise ValueError("Ошибка декодирования, не хватает частей секрета")
ValueError: Ошибка декодирования, не хватает частей секрета


## Дополнительные зависимости

module sympy
    можно установить командой (от администратора) pip3 install sympy
также этот сценарий лежит в файле dependencies.bat

## Кластер

Windows 10 Home Russian Версия 1809
Python 3.7.2 32bit
12GB RAM
Intel Core I7-4710MQ 2.50Ghz


## Автор

Кулаков Владислав Сергеевич КБ-501 © 2019

