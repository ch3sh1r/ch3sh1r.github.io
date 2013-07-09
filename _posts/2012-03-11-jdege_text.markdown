---
layout: post
title: "Класс Text"
abstract: "Короткое описание статьи."
tags: [cryptography, jdege.us]
---
> Вы видите перед собой вольный перевод [курса](http://jdege.us/
> crypto-python/index.html) за авторством Jeffrey Dege.
> Это значит, что все права на текст принадлежат автору, а в ошибках,
> скорее всего, стоит винить переводчика.

Программировать будем от конца к началу, то есть с целью сломать Monome-Dinome 
мы сначала напишем программу для шифрования открытого текста. Затем 
напишем программу для расшифрования по известному ключу. 

Практически всегда исследование нового типа шифра начинается с создания 
процедур шифрования и расшифрования. Это ведет к четкому пониманию работы 
шифра и позволяет проверить правильность этого понимания. Ну и, в конце 
концов, дает возможность создания неограниченного количества шифротекстов 
для анализа. 

И только завершив с шифровкой/расшифровкой, мы приступим к методам 
идентификации номеров строк и перевода шифротекста к легко 
взламываему виду.

Первый объект
-------------

Наконец-то мы переходим от разглагольствований к программированию. Для 
начала нам понадобится кусочек открытого текста, например вот такой: 
“As soon as we started programming, we found 
to our surprise that it wasn't as easy to get programs right as we had thought. 
Debugging had to be discovered. I can remember the exact instant when I realized that a 
large part of my life from then on was going to be spent in finding mistakes in my 
own programs.” Maurice Wilkes, конструктор EDSAC, 1949.

Запускайте свой интерпретатор и присвойте переменной `plaintext` эту цитату: 

    Python 2.4.3 (#1, Jan 21 2009, 01:10:13) 
    [GCC 4.1.2 20071124 (Red Hat 4.1.2-42)] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> plaintext = '''As soon as we started programming, we found to our surprise that it wasn't
    as easy to get programs right as we had thought.  Debugging had to be
    discovered.  I can remember the exact instant when I realized that a large
    part of my life from then on was going to be spent in finding mistakes in
    my own programs.'''
    >>> print plaintext
    As soon as we started programming, we found to our surprise that it wasn't
    as easy to get programs right as we had thought.  Debugging had to be
    discovered.  I can remember the exact instant when I realized that a large
    part of my life from then on was going to be spent in finding mistakes in
    my own programs.
    >>> 

Маленькое отступление. Автор и далее с успехом продолжает использовать интерпретатор 
напрямую. Мы же, как дети более светлого времени, воспользуемся достижениями мысли 
человеческой. А именно, перейдем на использование замены стандартного Python shell'а — 
[iPython](http://ipython.org/). На повествование это никак не повлияет, но даст 
нам всякие фишки вроде доступной истории команд и автокомплита по коду (а при 
желании и многие [другие](http://habrahabr.ru/blogs/python/117608/)). Итак, 

    >> ipython
    Python 2.7.2+ (default, Oct  4 2011, 20:03:08) 
    Type "copyright", "credits" or "license" for more information.

    IPython 0.10.2 -- An enhanced Interactive Python.
    ?         -> Introduction and overview of IPython's features.
    %quickref -> Quick reference.
    help      -> Python's own help system.
    object?   -> Details about 'object'. ?object also works, ?? prints more.

    In [1]: plaintext = '''As soon as we started programming, we found to our surprise that it wasn't
       ...:     as easy to get programs right as we had thought.  Debugging had to be
       ...:     discovered.  I can remember the exact instant when I realized that a large
       ...:     part of my life from then on was going to be spent in finding mistakes in
       ...:     my own programs.'''

    In [2]: print plaintext
    As soon as we started programming, we found to our surprise that it wasn't
        as easy to get programs right as we had thought.  Debugging had to be
        discovered.  I can remember the exact instant when I realized that a large
        part of my life from then on was going to be spent in finding mistakes in
        my own programs.

Как и в большинстве других языков программирования, в Python кавычки использованы для 
объявления строк. Но есть один нюанс — кавычки бывают двух типов. Тройные кавычки 
(`'''` и `"""`) используются для строк, содержащих символ новой строки `\n`.

Так вот, наш открытый текст содержится в объекте строкового типа с именем `plaintext`.
Теперь можно работать с этой строкой средствами Python, например привести 
к верхнему регистру или посчитать длину:

    In [3]: print plaintext.upper()
    AS SOON AS WE STARTED PROGRAMMING, WE FOUND TO OUR SURPRISE THAT IT WASN'T
        AS EASY TO GET PROGRAMS RIGHT AS WE HAD THOUGHT.  DEBUGGING HAD TO BE
        DISCOVERED.  I CAN REMEMBER THE EXACT INSTANT WHEN I REALIZED THAT A LARGE
        PART OF MY LIFE FROM THEN ON WAS GOING TO BE SPENT IN FINDING MISTAKES IN
        MY OWN PROGRAMS.

    In [4]: len(plaintext)
    Out[4]: 326

Это конечно хорошо, но наш `plaintext` не является строкой в интуитивном 
смысле. Разумеется в скором времени мы будем оперировать им как строкой, 
но сейчас необходима другая абстракция, способная вместить операции 
не принадлежащие к классу строковых. Попробуем определить класс `Text`, 
в который наша "нестрока" будет вписываться.

Обратите внимание — класс назван `Text` а не `Plaintext`, о котором идет 
речь. Это осмысленное действие — на данном этапе нет причин разделять эти 
понятия. А [привлечение](http://ru.wikipedia.org/wiki/Бритва_Оккама) 
новых сущностей без необходимости часто приводит к серьезным ошибкам. 

Итак, наш первый класс: 

    In [5]: class Text:
       ...:     def __init__(self, text):
       ...:         self.text = text
       ...:         
       ...:         

    In [6]: pt = Text(plaintext)

    In [7]: pt
    Out[7]: <__main__.Text instance at 0x98a29ac>

    In [8]: print pt.text
    As soon as we started programming, we found to our surprise that it wasn't
        as easy to get programs right as we had thought.  Debugging had to be
        discovered.  I can remember the exact instant when I realized that a large
        part of my life from then on was going to be spent in finding mistakes in
        my own programs.

Класс — это контейнер для данных и методов работы с этими данными. 
Наш `Text` состоит из переменной `text` и метода `__init__()`. `__init__()` — 
кодовое слово языка Python, это функция выполняющаяся при инициализации
объекта. То есть конструкция `pt = Text(plaintext)` присваивает переменной 
`text` класса `pt` значение переменной `plaintext`. 

А нужна вся это объектная ориентированность для возможности на лету 
конструировать много объектов со схожими свойствами:

    In [9]: pt2 = Text("Another text")

    In [10]: pt2.text
    Out[10]: 'Another text'

    In [11]: print pt.text
    As soon as we started programming, we found to our surprise that it wasn't
        as easy to get programs right as we had thought.  Debugging had to be
        discovered.  I can remember the exact instant when I realized that a large
        part of my life from then on was going to be spent in finding mistakes in
        part of my life from then on was going to be spent in finding mistakes in
        my own programs.

Но эти объекты существуют только в памяти интерпретатора и превратятся 
в историю сразу после выхода. Если же нам необходимо использовать их 
и в дальнейшем — стоит оформить их в модуль. 

Первый модуль
-------------

Модуль — это текстовый файл, содержащий код Python. Большая часть этого файла — 
описание классов/функций, и лишь маленький кусочек — инструкции по работе с ними. 
Чтобы создать модуль, откроем любимый текстовый редактор и запишем следущее: 

{% highlight python %}
class Text:
    def __init__(self, text):
        self.text = text
{% endhighlight %}

Теперь сохраняем все это под именем `crypto.py` в той же директории из которой 
запускаем iPython. Отныне командой `from crypto import *` мы возвращаем 
все наши объекты к жизни: 

    In [1]: from crypto import *

    In [2]: pt = Text("This is a text")

    In [3]: print pt.text
    This is a text

Осталось заметить, что такой метод импорта расходится с Дао Python, в котором 
пространства имен провозглашаются неплохой идеей (я вполне серьезен, попробуйте 
дать интерпретатору команду `import this`). Правильнее поступать так:

    In [1]: import crypto

    In [2]: pt = crypto.Text("This is another text")

    In [3]: print pt.text
    This is another text
            
Загрузка из файла
-----------------

Теперь нет необходимости перепечатывать определение нашего класса при каждом 
запуске интерпретатора. Хочется что-то похожее сделать и с открытым текстом. 
Было бы неплохо хранить его в файле и считывать при необходимости. 
Для претворения этого плана в жизнь файл `crypto.py` должен выглядеть 
как то так:

    class Text:
        def __init__(self, filename):
            self.text = ""

        def load(self, filename):
            fp = open(filename)
            self.text = fp.read()
            fp.close

        def __str__(self):
            return self.text

Здесь `__str__()` — еще одно ключевое слово, за которым следует описание 
поведения нашего класса в строковом контексте. А контекст — очередная 
фишка интерпретируемых языков программирования. Если коротко, это 
значит что язык не будет морочить голову программисту объемными описаниями, 
додумывая все очевидное за него.

Запишем исследуемую цитату в файл, назовем его `plain.txt` и опробуем 
на нем новый модуль: 

    In [4]: reload(crypto)
    Out[4]: <module 'crypto' from 'crypto.pyc'>

    In [5]: pt = crypto.Text("plain.txt")

    In [6]: print pt
    As soon as we started programming, we found to our surprise that it wasn't
    as easy to get programs right as we had thought.  Debugging had to be
    discovered.  I can remember the exact instant when I realized that a large
    part of my life from then on was going to be spent in finding mistakes in
    my own programs.
            
Форматирование вывода
---------------------

Изучаемый открытый текст содержит символы, часть которых никак не меняется 
при шифровании. Пробелы, пунктуация, escape-символы нам не интересны — 
значит в процессе исследования их следует отбросить. Кое-какие методы работы 
со строками нам уже известны, так напишем же с их помощью функцию конвертирования 
текста в необходимую форму: 

    class Text:
        def __init__(self, filename):
            self.load(filename)

        def load(self, filename):
            fp = open(filename)
            self.rawtext = fp.read()
            fp.close
            self.text = self.convert(self.rawtext)

        def convert(self, txt):
            rval = ""
            for c in txt.upper():
                if c.isalpha():
                    rval += c
            return rval

        def __str__(self):
            return self.text

Теперь метод `__str__()` возвращает все буквы из текста в верхнем регистре: 

    In [7]: reload(crypto)
    Out[7]: <module 'crypto' from 'crypto.pyc'>

    In [8]: pt = crypto.Text("plain.txt")

    In [9]: print pt
    ASSOONASWESTARTEDPROGRAMMINGWEFOUNDTOOURSURPRISETHATITWASNTASEASYTOGETPROGRAMSRIGHTASWEHADTHOUGHTDEBUGGINGHADTOBEDISCOVEREDICANREMEMBERTHEEXACTINSTANTWHENIREALIZEDTHATALARGEPARTOFMYLIFEFROMTHENONWASGOINGTOBESPENTINFINDINGMISTAKESINMYOWNPROGRAMS

С этим уже можно работать. Осталось только чуть-чуть подкорректировать вывод 
так, чтобы одновременно не мешало действиям алгоритма и не резало глаз человека. 
Пусть `__str__()` печатает группами по пять символов, по двенадцать групп 
в строку: 

    def __str__(self):
        rval = ""
        pos = 0
        for c in self.text:
            rval += c
            pos += 1
            if pos % 60 == 0:
                rval += '\n'
            elif pos % 5 == 0:
                rval += " "
        return rval

Компактно и внушает уважение наблюдателям.

    In [10]: reload(crypto)
    Out[10]: <module 'crypto' from 'crypto.pyc'>

    In [11]: pt = crypto.Text("plain.txt")

    In [12]: print pt
    ASSOO NASWE START EDPRO GRAMM INGWE FOUND TOOUR SURPR ISETH ATITW ASNTA
    SEASY TOGET PROGR AMSRI GHTAS WEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
    REDIC ANREM EMBER THEEX ACTIN STANT WHENI REALI ZEDTH ATALA RGEPA RTOFM
    YLIFE FROMT HENON WASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOW NPROG
    RAMS
            
Первая программа
----------------

Вначале курса упоминалось, что есть еще один способ запуска — как 
обычного исполняемого файла. Он выполнен как передача интерпретатору файла 
с исходным кодом в виде аргумента из командной строки. А в некоторых 
средах можно обойтись и без упоминания интерпретатора, оставив его координаты 
в исходниках.

Такое можно проделать и с нашим `crypto.py`, правда он пока не способен на 
внятные самостоятельные действия. По вызову он просто создает описанные классы 
и отключается. Вроде бы осталось только описать необходимые действия, 
но тогда они будут выполняться каждый раз как мы импортируем программу в 
качестве модуля. Чтобы такого не происходило, введем в использование 
еще парочку кодовых слов Python: 

    if __name__ == "__main__":
        txt = Text('plain.txt')
        txt.load('plain.txt')
        print txt

Переменная `__main__` принимает значение `"__name__"` только когда 
файл исполняется интерпретатором. Это и позволяет использовать написанные 
программы как библиотеки а библиотеки как программы. 

    >> python crypto.py
    ASSOO NASWE START EDPRO GRAMM INGWE FOUND TOOUR SURPR ISETH ATITW ASNTA
    SEASY TOGET PROGR AMSRI GHTAS WEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
    REDIC ANREM EMBER THEEX ACTIN STANT WHENI REALI ZEDTH ATALA RGEPA RTOFM
    YLIFE FROMT HENON WASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOW NPROG
    RAMS
    >>       

И добавим очевидное — пусть имя обрабатываемого файла передается как параметр: 

    #!/usr/bin/env python
    import sys

    #...тут все без изменений...

    if __name__ == "__main__":
        if len(sys.argv) == 2:
            txt = Text(sys.argv[1])
        else:
            print "Usage: crypto.py <filename>"
            sys.exit(1)
        print txt

Итак, всего два нововведения. Во-первых, мы добавили юниксовый 
"shebang" комментарий, который и позволяет в Unix запускать программу не передавая 
ее интерпретатору напрямую. Для Windows это не обязательно. 
Там решающим фактором становится расширение, и файл оканчивающийся на 
`.py` будет автоматически передан интерпретатору. 

Во-вторых, импортирован модуль `sys`. Это стандартный модуль который 
поставляется с каждой установкой Python. Он содержит функции и переменные 
для создания интерфейса по работе с операционной системой. В нашем случае, 
он использован для получения аргументов командной строки при запуске 
нашей программы и для выхода при получении незапланированных аргументов. 
Переменная `sys.argv` указывает на массив строк, нулевой элемент которого — 
имя запущенной программы, а первый содержит единственный в нашем случае 
аргумент. 

    >> python ./crypto.py
    Usage: crypto.py <filename>
    >> python crypto.py plain.txt 
    ASSOO NASWE START EDPRO GRAMM INGWE FOUND TOOUR SURPR ISETH ATITW ASNTA
    SEASY TOGET PROGR AMSRI GHTAS WEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
    REDIC ANREM EMBER THEEX ACTIN STANT WHENI REALI ZEDTH ATALA RGEPA RTOFM
    YLIFE FROMT HENON WASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOW NPROG
    RAMS
    >> 

