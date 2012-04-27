---
layout: post
title: "Криптоанализ II"
tags: [translate, cryptography, jdege.us]
---
> Вы видите перед собой вольный перевод [курса](http://jdege.us/
> crypto-python/index.html) за авторством Jeffrey Dege.
> Это значит, что все права на текст принадлежат автору, а в ошибках,
> скорее всего, стоит винить переводчика.

Decrypting with an incorrect key?
---------------------------------

If we know the row digits of the key, we still do not know the order of the column digits, or the ordering of the letters within the grid. We cannot decipher the text, knowing only the row digits. But happens when we try to decrypt a text using a key that is correct only in having the right row digits? We'll have the wrong values for every output - but, we'll have combined the right digits together.

As an example, let's look at the first few letters of our plaintext and how they encrypt into cyphertext.

    A	S	S	O	O	N	A	S	V	E
    6	34	34	28	28	27	6	34	1	21

Now, let's decrypt them with key for which the row digits are incorrect.

    63	4	3	4	2	82	82	7	63	4	1	2	1
    U	E	D	E	C	L	L	G	U	E	B	C	B

There's no immediately visible relationship between this incorrect decryption and our plaintext. Notice that we don't even get the same number of letters. Now let's try decrypting with an incorrect key that does have the correct row digits.

    6	34	34	28	28	27	6	34	1	21
    C	V	V	M	M	P	C	V	B	K

And here, we have a relationship. It's more clear if we lay the two texts side-by-side.

    A	S	S	O	O	N	A	S	V	E
    C	V	V	M	M	P	C	V	B	K

The result of decrypting with a key that is incorrect in all respects save that it has the correct row digits is a simple substitution cipher of the plaintext. We don't, of course, know the key to this substitution, but simple substitution ciphers we know how to break.

We have, by identifying the row digits, converted a complex cipher into a simple one that can be broken without trouble.

Simplifying complex ciphers
---------------------------

Converting a complex cipher into a simpler one, in this way, is a 
very powerful technique. It could be argued that it is the primary technique for 
dealing with complex ciphers. It has been applied in many circumstances, though the 
specifics differ depending upon the construction of the cipher.

* If you have a checkboard cipher, with single digits for row and column 
  indexes, by decrypting with any arbitrary checkboard you will end up with a simple 
  substitution cipher.
* In a mixed alphabet Vigenere, the alphabets are created by 
  shifting a base alphabet. If you have identified the number of alphabets, and 
  can determine the amount by which they need to be shifted for their frequency distributions 
  to match, you can remove those shifts, converting the Vig into a simple 
  substitution cipher.
* An autokey cipher starts encrypting with a keyword like the Vigenere
  , but when the keyword has been exhausted, instead of repeating it the cipher 
  starts using the first letter of the plaintext as the key. Decryting this with 
  a null key of the proper length converts the ciphertext into a Vigenere with twice 
  the period of the original. They key to this converted cipher is the key 
  of the original cipher followed by its inverse. 

Removing the row digits
-----------------------

All we need to to try this out in our code is to make a 
modification to the MonomeDinome class so that we can construct one with a specified set 
of row digits. It's a minor alteration:

    class MonomeDinome:
        def __init__(self, dkw, lkw = ""):
            self.digitsKey = self.digitsScramble(dkw)
            self.lettersKey = self.lettersScramble(lkw)

        def digitsScramble(self, dkw):
            dkl = list((dkw.upper() + "ZZZZZZZZZZ")[0:10])
        
            for i in "0123456789":
                if i not in dkl:
                    pos = self.findLowestLetter(dkl)
                    if pos != -1:
                        dkl[pos] = i
        
            return "".join(dkl)

First, we make the second argument optional. Then we check to see if 
a digit is already in our digitskey list before we insert it. With this 
change, if we pass an initialization string with digits in it, instead of 
a keyword, those digits will be placed at the beginning of the key—which is 
where the row digits are.

    >>> pt = crypto.Text('plaintext.txt')
    >>> print pt
    ASSOO NASWE START EDPRO GRAMM INGWE FOUND TOOUR SURPR ISETH ATITW ASNTA
    SEASY TOGET PROGR AMSRI GHTAS WEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
    REDIC ANREM EMBER THEEX ACTIN STANT WHENI REALI ZEDTH ATALA RGEPA RTOFM
    YLIFE FROMT HENON WASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOW NPROG
    RAMS
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
    >>> mdc = crypto.MdCrack(ct)
    >>> mdc.rowDigits()
    ['01', '04', '07', '09', '14', '18', '23', '47', '49', '58', '67', '78', '89']
    >>> fc = crypto.Freq(ct.get())
    >>> print fc
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
    >>> md2 = crypto.MonomeDinome('23')
    >>> pt2 = md2.decrypt(ct)
    >>> print pt2
    ETTPP OETBK TUERU KHQRP LREDD AOLBK IPVOH UPPVR TVRQR ATKUM EUAUB ETOUE
    TKETY UPLKU QRPLR EDTRA LMUET BKMEH UMPVL MUHKF VLLAO LMEHU PFKHA TGPBK
    RKHAG EORKD KDFKR UMKKX EGUAO TUEOU BMKOA RKECA ZKHUM EUECE RLKQE RUPID
    YCAIK IRPDU MKOPO BETLP AOLUP FKTQK OUAOI AOHAO LDATU ENKTA ODYPB OQRPL
    REDT
    >>>

That looks like anything but clear text, but if you compare it to the 
plaintext, it is clear that it's a simple substitution cipher. This 
isn't a tutorial on simple substitution ciphers, so I'm not 
going to explain how to crack this. There are plenty of sources of information 
on how to do that. Or, if you're lazy, you 
can simply feed it through Decrypto 8.5. In 9.992 seconds, it gives:

    AS SOON AS WE STARTED PROGRAMMING WE FOUND TO OUR SURPRISE THAT IT WASN T
    A SEASY TOGET PROGRAMS RIGHT AS WE HAD THOUGHT DEBUGGING HAD TO BE
    DISCOWEREDICAN REMEMBER THE EXACT INSTANT WHEN I REALIVED THAT ALARGE PART
    OF MY LIFE FROM THEN ON WAS GOING TO BE SPENT IN FINDING MISTAKES IN MY OWN
    PROGRAMS            
            
