---
layout: post
title: "Класс Encryption"
abstract: "Разбор модуля шифрования."
tags: [cryptography, jdege.us]
---
> Это вольный перевод
> [курса статей](http://jdege.us/crypto-python/index.html)
> за авторством Jeffrey Dege.

### Содержание

1. [Введение](/posts/jdege)
2. [Python](/posts/jdege-python)
3. [Криптография](/posts/jdege-cryptography)
4. [Класс Text](/posts/jdege-text)
5. [**Класс Encryption**](/posts/jdege-encryption)
6. [Криптоанализ I - Числа](/posts/jdege-cryptanalysis-1)
7. [Криптоанализ II - Фокус](/posts/jdege-cryptanalysis-2)
8. [Криптоанализ III - Точность](/posts/jdege-cryptanalysis-3)
9. [Программирование](/posts/jdege-programming)

Now that we have a class to contain our plaintext, it's time to create a class to encrypt our plaintext. We'll begin with the code necessary to construct the key, and then add the code to do the encryption and decryption using the key.

### Ключ

Since our new class will be closely related to our Text class, we might as well create it in the same file.

You'll notice that class that doesn't actually do anything, yet. This is generally a good idea. As Grady Booch once wrote: “A complex system that works is invariably found to have evolved from a simple system that worked ... A complex system designed from scratch never works and cannot be patched up to make it work. You have to start over, beginning with a working simple system.” If you make too many changes at once, it can be hard to figure out which change didn't work. When working in an environment where making a change then running a test is fast and easy, make the smallest change you can that will move you towards the system you are trying to build, test that, then make the next smallest change.

```python
class MonomeDinome:
    def __init__(self, dkw, lkw):
        self.digitsKeyword = dkw
        self.lettersKeyword = lkw

    def __str__(self):
        return "digitsKey = " + self.digitsKey + "\n" + \
                "lettersKey = " + self.lettersKey
```

```python
>>> import crypto
>>> md = MonomeDinome('fred', 'wilma')
>>> print md
digitsKey = fred
lettersKey = wilma
>>>
```

In a Monome Dinome cipher, both the digits and the letters need to be scrambled. It's normal practice, in ACA ciphers, to do this with keywords. The digits are scrambled by writing the digits 0 through 9 under the letters of the keyword in alphabetic order, then reading them off left-to-right.

```
S E C R E T W O R D
7 2 0 5 3 8 9 4 6 1
```

To scramble the keyword, we make use of a new Python data type, the list. A list is simply a sequence of objects. They differ from strings in two respects: a string is a sequence of letters, a list can contain objects of any type, and a list can be modified, where a string cannot.

It may not have been obvious, in what we've done so far, but strings in Python are immutable. Once they're created, they cannot be changed. You cannot take the string "bench" and change the third letter to "a" to create the string "beach". Instead, you have to create a new string from the parts of the old string you wanted to keep, plus the parts you wanted to add.

Earlier, it appeared as if we were building up a string by appending letters to its end with string += letter. What we were actually doing was creating a new string, made up of the old string plus the extra letter, and throwing the old string away. This is can be a very inefficient way of writing code, when the strings in question are very long.

```python
class MonomeDinome:
    def __init__(self, dkw, lkw):
        self.digitsKey = self.digitsScramble(dkw)
        self.lettersKey = self.lettersScramble(lkw)

    def digitsScramble(self, dkw):
        dkl = list((dkw.upper() + "ZZZZZZZZZZ")[0:10])

        for i in "0123456789":
            pos = self.findLowestLetter(dkl)
            if pos != -1:
                dkl[pos] = i

        return "".join(dkl)

    def findLowestLetter(self, lst):
        pos = -1
        lowest = ''

        for i in range(len(lst)):
            if lst[i].isapha():
                if (lowest == '') or (lst[i] < lowest):
                    lowest = lst[i]
                    pos = i
        return pos

    def lettersScramble(self, lkw):
        rlist = []
        for a in lkw.upper():
            if a == "W":
                a = "V"
            if a == "J":
                a = "I"
            if not a in rlist:
                rlist.append(a)

        for a in "ABCDEFGHIKLMNOPQRSTUVXYZ":
            if not a in rlist:
                rlist.append(a)

        return "".join(rlist)

    def __str__(self):
        return "digitsKey = " + self.digitsKey + "\n" + \
                "lettersKey = " + self.lettersKey
```

To scramble the digits 0 through 9, we create a list of 10 letters from our keyword. Then for each digit, we find the lowest letter that remains in our list, and replace it with that digit.

Lists can't be directly printed, the way strings can be. But if the elements of a list are strings (and ours are, that's why we're doing for i in "0123456789" instead of for i in range(10)), we can convert a list into a string using a string object's join() method. This concatenates all of the elements of the list together, using the string value as a separator. "".join(lst) creates a string out of all of the elements of lst separated by the empty string.

Scrambling an alphabet is simpler. We simply take the letters from the keyword, ignoring duplicates, and then append the remaining letters from the alphabet. The only unusual thing in this case is that we treat J as if it were I and W as if it were V.


```python
>>> md = crypto.MonomeDinome('fred', 'wilma')
>>> print md
digitsKey = 2310456789
lettersKey = VILMABCDEFGHKNOPQRSTUXYZ
>>>
```

### Шифрование

We're finally ready to start encrypting. For that, we'll need some plaintext. Fortunately, we already have a class designed to handle plaintext, sowe'll stub out the start of an encryption routine:

```python
class MonomeDinome:
    [...]
    def encrypt(self, pt):
        return pt;
```

A quick test to ensure that our framework changes are correct:

```python
>>> pt = crypto.Text('plaintext.txt')
>>> md = crypto.MonomeDinome('fred', 'wilma')
>>> ct = md.encrypt(pt)
>>> print ct
ASSOO NASWE START EDPRO GRAMM INGWE FOUND TOOUR SURPR ISETH ATITW ASNTA
SEASY TOGET PROGR AMSRI GHTAS WEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
REDIC ANREM EMBER THEEX ACTIN STANT WHENI REALI ZEDTH ATALA RGEPA RTOFM
YLIFE FROMT HENON WASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOW NPROG
RAMS
>>>
```

And now we're ready to start writing the encryption routine itself. We'll start by creating a new list containing the letters from the plaintext, and then creating a cyphertext object that containst those letters. We're using a new Python feature, changing Text's __init__() to take an optional argument, so that we can construct one without passing a filename. We also added set() and get() methods.

```python
class Text:
    def __init__(self, filename = 0):
        if filename:
            self.load(filename)

    [...]

    def set(self, lst):
        self.text = "".join(lst)

    def get(self):
        return self.text

class MonomeDinome:
    [...]
    def encrypt(self, pt):
        rlist = []
        for c in pt.get():
            rlist.append(c)
        ct = Text()
        ct.set(rlist)
        return ct
```

With that working, we'll populate rlist with the proper digits according to our key:

```python
def encrypt(self, pt):
    rlist = []
    for c in pt.get():
        if c == 'J':
            c = 'I'
        if c == 'W':
            c = 'V'
        pos = self.lettersKey.find(c)

        if pos == -1:
            continue
        elif pos < 8:
            rlist.append(self.digitsKey[pos+2])
        elif pos < 16:
            rlist.append(self.digitsKey[0])
            rlist.append(self.digitsKey[pos-8+2])
        elif pos < 24:
            rlist.append(self.digitsKey[1])
            rlist.append(self.digitsKey[pos-16+2])
        else:
            continue

    ct = Text()
    ct.set(rlist)
    return ct
```

```python
>>> reload(crypto)
<module 'crypto' from 'crypto.py'>
>>> pt = crypto.Text('plaintext.txt')
>>> md = crypto.MonomeDinome('fred', 'wilma')
>>> ct = md.encrypt(pt)
>>> print ct
63434 28282 76341 21343 56303 52192 93028 24306 55027 24121 20283 62793
52828 36303 43630 29300 34213 52563 50351 63427 35634 21634 38352 82421
35293 02824 30653 43002 42535 63412 12569 35252 83624 25359 21736 24240
27242 56935 28721 90348 28121 30219 08627 30215 21572 13035 25212 13768
35027 34356 27351 25212 70302 16403 92193 52563 56463 02421 29630 35282
05384 02021 20302 85352 52127 28271 63424 28027 24352 87213 42921 27350
27200 27902 72450 34356 26213 40275 38281 27293 02824 30653 4
>>>
```

### Расшифрование

With the plumbing we have created for encryption already in place, adding decryption should be easy.

```python
def decrypt(self, ct):
    rlist = []
    saved = -1
    for c in ct.get():
        pos = self.digitsKey.find(c)
          if pos == 0 or pos == 1:
            saved = pos
        elif saved == -1:
            pos = pos - 2
            rlist.append(self.lettersKey[pos])
        else:
            pos = pos - 2 + 8 + saved * 8
            rlist.append(self.lettersKey[pos])
            saved = -1

    pt = Text()
    pt.set(rlist)
    return pt
```

And it is.

```python
>>> reload(crypto)
<module 'crypto' from 'crypto.py'>
>>> pt = crypto.Text('plaintext.txt')
>>> md = crypto.MonomeDinome('fred', 'wilma')
>>> ct = md.encrypt(pt)
>>> print ct
63434 28282 76341 21343 56303 52192 93028 24306 55027 24121 20283 62793
52828 36303 43630 29300 34213 52563 50351 63427 35634 21634 38352 82421
35293 02824 30653 43002 42535 63412 12569 35252 83624 25359 21736 24240
27242 56935 28721 90348 28121 30219 08627 30215 21572 13035 25212 13768
35027 34356 27351 25212 70302 16403 92193 52563 56463 02421 29630 35282
05384 02021 20302 85352 52127 28271 63424 28027 24352 87213 42921 27350
27200 27902 72450 34356 26213 40275 38281 27293 02824 30653 4
>>> pt2 = md.decrypt(ct)
>>> print pt2
ASSOO NASVE START EDPRO GRAMM INGVE FOUND TOOUR SURPR ISETH ATITV ASNTA
SEASY TOGET PROGR AMSRI GHTAS VEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
REDIC ANREM EMBER THEEX ACTIN STANT VHENI REALI ZEDTH ATALA RGEPA RTOFM
YLIFE FROMT HENON VASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOV NPROG
RAMS
>>>
```
