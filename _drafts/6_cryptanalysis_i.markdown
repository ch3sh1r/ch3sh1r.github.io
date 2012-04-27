---
layout: post
title: "Криптоанализ I"
tags: [translate, cryptography, jdege.us]
---
> Вы видите перед собой вольный перевод [курса](http://jdege.us/
> crypto-python/index.html) за авторством Jeffrey Dege.
> Это значит, что все права на текст принадлежат автору, а в ошибках,
> скорее всего, стоит винить переводчика.


Now that we have our encryption and decryption routines, we are ready to start creating some cryptanalytic tools. The most important of these, and the oldest historically, is the simple frequency count.

The Frequency count
-------------------

We'll start by adding to our crypto module a simple frequency counting class. The input it takes is pretty flexible. Any object that can be enumerated with Python's a in b syntax should work.

    from operator import itemgetter

    [...]

    class Freq:
        def __init__(self, txt):
            self.count = self.doCount(txt)
        
        def doCount(self, txt):
            dict = {}
            for c in txt:
                if c in dict:
                    dict[c] += 1
                else:
                    dict[c] = 1
            return dict

        def get(self):
            return self.count

        def __str__(self):
            items = self.count.items()
            items.sort(key=itemgetter(1), reverse=True)
            rval = []
            for i in items:
                rval.append(i.__str__())
             
            return "\n".join(rval)

We implement it using a new Python data structure, a dictionary. A dictionary works something like an array, except that it's arguments are arbitrary objects, not just a range of integers. We store the count for the letter 'A' in the dictionary indexed by the letter 'A'. We use a bit of Python wizardry to sort the dictionary by reverse value. With it written, we can play around a bit with it, in the interpreter, and see how it handles different types of input.

    >>> reload(crypto)
    <module 'crypto' from 'crypto.py'>
    >>> pt = crypto.Text('plaintext.txt')
    >>> md = crypto.MonomeDinome('fred', 'wilma')
    >>> ct = md.encrypt(pt)
    >>> fc = crypto.Freq(pt.get())
    >>> print fc
    ('E', 25)
    ('T', 24)
    ('A', 23)
    ('O', 18)
    ('N', 18)
    ('S', 18)
    ('R', 18)
    ('I', 17)

    [...]

    >>> fc2 = crypto.Freq(ct.get())
    >>> print fc2
    ('2', 97)
    ('3', 70)
    ('5', 44)
    ('0', 40)
    ('4', 35)
    ('1', 33)
    ('6', 29)
    ('8', 24)
    ('7', 23)
    ('9', 16)
    >>> 

In the frequency count of our plaintext, we see the familiar patterns we expect to see, with the expected peaks at the expected values. If our ciphertext were the product of a simple substitution cipher, we'd see the same peaks, simply for different values. Since our cipher is not simple substitution, our frequency count shows an unfamiliar pattern.

The n-gram frequency count
--------------------------

Often, you can tell more about a cipher by looking not at how often 
characters appear, but at what characters they appear next to. This is the 
contact behavior, and it can be extracted from a biliteral frequency count. That 
is, a count of how often pairs of characters appear. We could create 
another class for biliteral counts, and a third for triliteral counts, but it 
makes most sense to simply extend our current class to allow for arbitrary lengths.

    class Freq:
        def __init__(self, txt, n = 1):
            self.count = self.doCount(txt, n)
        
        def doCount(self, txt, n):
            dict = {}
            for i in range(n, len(txt)+1):
                s = txt[i-n:i]
                if s in dict:
                    dict[s] += 1
                else:
                    dict[s] = 1
            return dict

Again, we can play around with it in the Python interpreter:

    >>> print crypto.Freq("ABCDABAD")
    ('A', 3)
    ('B', 2)
    ('D', 2)
    ('C', 1)
    >>> print crypto.Freq("ABCDABAD", 2)
    ('AB', 2)
    ('AD', 1)
    ('BC', 1)
    ('CD', 1)
    ('DA', 1)
    ('BA', 1)
    >>> print crypto.Freq("ABCDABAD", 3)
    ('ABA', 1)
    ('ABC', 1)
    ('CDA', 1)
    ('DAB', 1)
    ('BCD', 1)
    ('BAD', 1)
    >>> print crypto.Freq(ct.get(), 2)
    ('21', 25)
    ('35', 24)
    ('02', 21)
    ('30', 18)
    ('28', 18)
    ('27', 18)
    ('34', 18)
    ('52', 17)
    [...]
    >>>
            
The Row Digits
--------------

We have spent quite a lot of time, so far, in building up 
tools, but we've done nothing, as of yet, that might 
be construed as an attack on the Monome-Dinome Cipher. It's 
time to change that.

Let's look at how the cipher works, again:

    -	1	0	4	5	6	7	8	9
    -	V	I	L	M	A	B	C	D
    2	E	F	G	H	K	N	O	P
    3	Q	R	S	T	U	X	Y	Z

And let's look at some of the plaintext and ciphertext, preserving spaces 
between the plaintext characters. Pay particular attention to how the row digits lay out 
in the ciphertext.

    A	S	S	O	O	N	A	S	V	E	 	 	 	 
    6	34	34	35	28	28	27	6	1	21	 	 	 	 

Notice how the row digits only show up as the start of pairs? In 
this text, our row digits are 2 and 3. We never see one 
of our row digits doubled, and we never see one of our row digits 
adjacent to the other row digit. In fact, given the way the Monome
-Dinome cipher works, we will never see a row digit adjacent to a 
row digit. We can use that fact to significantly reduce the number of possibile 
row digits. Which digits appear next to each other in the ciphertext is revealed 
in the biliteral frequency count, so we'll create a class that will 
use that to determine which sets of digits could be row digits.

    class MdCrack:
        def __init__(self, txt):
            self.txt = txt;
            self.fc1 = Freq(txt.get(), 1)
            self.fc2 = Freq(txt.get(), 2)

        def rowDigits(self):
            digits = self.fc1.get().keys()
            digits.sort()
            
            paireddigits = self.fc2.get().keys()
            paireddigits.sort()
            
            pairs = {}
            
            for i in digits:
                for j in digits:
                    pairs[i + j] = 1

            for i in digits:
                pairs[i + i] = 0

            for i in digits:
                for j in digits[int(i):]:
                    if i + j in paireddigits:
                        pairs[i + j] = 0
                    if j + i in paireddigits:
                        pairs[j + i] = 0

            rval = []
            for i in digits:
                for j in digits[int(i):]:
                    if pairs[i + j] == 1:
                        rval.append(i + j)

            return "\n".join(rval)

Since order is immaterial, there are ten-choose-two or forty-
five possibilities for the row digits. The longer the ciphertext, the more of 
the possibilities that can be eliminated. Our ciphertext is relatively short, so there 
are still thirteen possibilities left.

    >>> mcd = crypto.MdCrack(ct)
    >>> print mcd.rowDigits()
    ['01', '04', '07', '09', '14', '18', '23', '47', '49', '58', '67', '78', '89']
    >>> print len(mcd.rowDigits())
    13
    >>> 

Thirteen is better than forty-five, but it's still enough to 
be tedious enough to go through by hand. Which of these is most likely
? We could look at the uniliteral frequency count. That was, if you remember:

    >>> fc2 = crypto.Freq(ct.get())
    >>> print fc2
    ('2', 97)
    ('3', 70)
    ('5', 44)
    ('0', 40)
    ('4', 35)
    ('1', 33)
    ('6', 29)
    ('8', 24)
    ('7', 23)
    ('9', 16)
    >>> 

We already know, in this case, that the row digits are 2 and 
3. And these are, not surprisingly, at the top of our list
. But more than that, all of the other possible pairs include at least 
one digit that is fairly far down on the list. That is the usual 
pattern—the row digits are the only high-frequency pair that isn't eliminated 
by our filtering.

Still, suppose we're right? How does that help us? 

