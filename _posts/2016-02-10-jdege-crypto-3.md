---
layout: post
title: "Криптоанализ III - Точность"
abstract: ""
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
5. [Класс Encryption](/posts/jdege-encryption)
6. [Криптоанализ I - Числа](/posts/jdege-cryptanalysis-1)
7. [Криптоанализ II - Фокус](/posts/jdege-cryptanalysis-2)
8. [**Криптоанализ III - Точность**](/posts/jdege-cryptanalysis-3)
9. [Программирование](/posts/jdege-programming)

There was some guesswork involved, in our solution. We had thirteen possibilities for our row digit pairs. We guessed that the highest frequency possibility would be correct, and that proved to be the case. That will often, but not always, true. Is there more we can do?

One thing that comes to mind is trying all of them. We could do a trial decrypt with every pair that hadn't been ruled out as impossible. The problem, then, is identifying which is most likely to be correct. One of the results will be a simple substitution, the others will be incorrect decryptions. That's not a distinction that is easy to make by eye. Can we write a routine that can distinguish one from the other? Of course there is.

What we're talking about is a scoring algorithm, and these are an essential part of computer cryptography. A computer can make a great many trials—thousands or sometimes millions in a second—but that doesn't help much if the computer can't distinguish between a good or a bad result. What we need is a method by which the computer can measure how close the result is to what we expect to find.

Programs like DeCrypto contain such routines, measures of how close a trial decrypt is to ordinary text. In our case, these won't work, because our correct result isn't ordinary text. What we need is a measure that will distinguish a simple substitution from a more complicated manipulation of the text. Exactly such a measure is the Index of Coincidence, first described by William Friedman in 1922.

The Index of Coincidence

The Index of Coincidence is the probability that two characters chosen at random from the text will be the same. For a random text, this is simply 1/c, where c is the number of distinct characters in the text. For a non-random text, it is:

$$ IC = \frac{\sum_{i=1}^c n_i (n_i - 1)}{N (N - 1)} $$

Where $c$ is the number of distinct characters in the text, ni is the number of times the i'th character appears, and N is the total number of characters.

The $IC$ of random text consisting of 26 distinct letters is 0.0385. The IC of ordinary english text, with upper and lower case collapsed, and spaces and punctuation stripped (so that there are 26 distinct characters) is 0.0665. We, however, are working with an alphabet of 24 letters, so our random text would have an IC of 0.0417, and our plain text would have an IC of 0.0661.

The significant fact, when trying to understand the Index of Coincidence, is that simple substitution doesn't change it. If you change all of the E's to Q's and all the Q's to E's, that won't change the IC. The IC is determined by the number of occurances of each distinct letter, not by their value.

What this means is that the IC can be used to distinguish between a ciphertext that is the product of a simple substitution and one that is the product of a more complicated cipher. The IC is widely used in solving Vigenere's and other polyalphabetic ciphers. And because a trial decryption using the correct row digits yields a simple substittion, while a trial decryption using incorrect row digits does not, we can use the IC to test whether a guess at the row digits is correct, without having to try to decrypt the substitution cipher.

Coding The Index of Coincidence

We calculate the IC from the frequency count, so we can easily implement it by adding a method to our Freq class.

class Freq:

    [...]
    
    def calc_ic(self):
        num = 0.0
        den = 0.0
        for val in self.count.values():
            i = val
            num += i * (i - 1)
            den += i
        
        if (den == 0.0):
            return 0.0
        else:
            return num / ( den * (den - 1))
    
            
A little bit of playing around suggests that the IC of a decrypt with the correct row digits is distinguishable.


>>> print crypto.Freq(crypto.MonomeDinome('23').decrypt(ct).get()).calc_ic()
0.0631451123254
>>> print crypto.Freq(crypto.MonomeDinome('24').decrypt(ct).get()).calc_ic()
0.0997911348341
>>> print crypto.Freq(crypto.MonomeDinome('13').decrypt(ct).get()).calc_ic()
0.111827911502
>>> print crypto.Freq(crypto.MonomeDinome('01').decrypt(ct).get()).calc_ic()
0.101171141116
>>> print crypto.Freq(crypto.MonomeDinome('99').decrypt(ct).get()).calc_ic()
0.126556576496
>>> 
            

Next, let's add a method to MdCrack that calculates the IC for all of the valid row digits, sorted in increasing order, and a method that returns the row digits that have the lowest IC.

class MdCrack:

    [...]
    
    def ics(self):
        rdict = {}
        for i in self.rowDigits():
            md = MonomeDinome(i)
            pt = md.decrypt(self.txt)
            fc = Freq(pt.get())
            rdict[i] = fc.calc_ic()
        
        items = rdict.items()
        items.sort(key=itemgetter(1), reverse=False)
        rval = []
        for i in items:
            rval.append(i)
        return rval

    def ic(self):
        return self.ics()[0]
            
With this, we can play around with different keys, to see if the same pattern holds.


>>> import crypto
>>> pt = crypto.Text('plaintext.txt')
>>> print crypto.MdCrack(crypto.MonomeDinome('01', '').encrypt(pt)).ics()
[('01', 0.063145112325440192), ('23', 0.092936651675790072), ('29', 0.10146295422369042), ('09', 0.10920029042630433), ('35', 0.11693000761866905), ('48', 0.11713055855670457), ('57', 0.1174892971964485), ('47', 0.1179575642573745), ('37', 0.11951373016661698), ('59', 0.1201949860724234), ('39', 0.12222222222222222), ('79', 0.12293407613741876)]
>>> print crypto.MdCrack(crypto.MonomeDinome('03', '').encrypt(pt)).ics()
[('03', 0.063145112325440192), ('12', 0.092936651675790072), ('19', 0.10146295422369042), ('09', 0.10920029042630433), ('25', 0.11693000761866905), ('48', 0.11713055855670457), ('57', 0.1174892971964485), ('47', 0.1179575642573745), ('27', 0.11951373016661698), ('59', 0.1201949860724234), ('29', 0.12222222222222222), ('79', 0.12293407613741876)]
>>> print crypto.MdCrack(crypto.MonomeDinome('59', '').encrypt(pt)).ics()[('59', 0.063145112325440192), ('01', 0.092936651675790072), ('08', 0.10146295422369042), ('58', 0.10920029042630433), ('13', 0.11693000761866905), ('27', 0.11713055855670457), ('36', 0.1174892971964485), ('26', 0.1179575642573745), ('16', 0.11951373016661698), ('38', 0.1201949860724234), ('18', 0.12222222222222222), ('68', 0.12293407613741876)]
>>> print crypto.MdCrack(crypto.MonomeDinome('01', 'zyxvutsrqponmlkihgfedcba').encrypt(pt)).ics()
[('01', 0.063145112325440192), ('79', 0.10512567481279388), ('56', 0.1071409419966038), ('25', 0.11030507800854616), ('29', 0.11279972399516991), ('47', 0.11660072796328533), ('02', 0.1175082586125531), ('34', 0.12093816111897478), ('78', 0.12099896138230573), ('48', 0.12425612224708156), ('38', 0.12496594314776133), ('24', 0.12511942959001782), ('36', 0.12533674261449249), ('23', 0.12561702127659574), ('26', 0.12862745098039216), ('28', 0.12882269503546098)]
>>> print crypto.MdCrack(crypto.MonomeDinome('01', 'etaoinshrdlu').encrypt(pt)).ics()
[('01', 0.063145112325440192), ('37', 0.09403827619441002), ('45', 0.094623655913978491), ('49', 0.094875723969560816), ('89', 0.096774996267356792), ('79', 0.096883468834688347), ('15', 0.098281214507458164), ('59', 0.098413781340610615), ('58', 0.098589130168077535)]
>>> 
            

Changes to the row digits make no difference at all, all of the IC's are the same. Changes to the alphabet layout do change the IC's that result from incorrect row digits (though the IC for the correct row digits doesn't change, of course, it is always the same as that of the plaintext.) The IC of incorrect rows seems to be higher than that of regular text because the row digits generally appear with high frequency. If we treat them as a column digit, this results in certain characters appearing far more often than they would in ordinary text. This pattern appears even when we specifically choose our key to put the high-frequency letters in the first row, to minimize the appearance of the row digits. It appears that we have a workable test.

