---
layout: post
title: "Создание программ"
tags: [translate, cryptography, jdege.us]
---
> Вы видите перед собой вольный перевод [курса](http://jdege.us/
> crypto-python/index.html) за авторством Jeffrey Dege.
> Это значит, что все права на текст принадлежат автору, а в ошибках,
> скорее всего, стоит винить переводчика.

We've created code that can encrypt and decrypt Monome-Dinome ciphers, 
and that can convert a ciphertext into a simple substitution. All of this code
, though, has been run through the Python interpreter. It would be more 
convenient, though, if we had some actual programs, that could be run 
independtly. As we saw earlier, this isn't difficult to do.

The first question is how many programs do we want? A separate program for 
each of the three functions? Or one program that does all three, depending 
upon command-line arguments to choose between them? The second question is how 
many files, and how to distribute the functionality among them.

Рефакторинг
----------

Our answer to the first question, above is that we'll want just 
one program. Our answer to the second question is that we'll want two files.

The process of reorganizing a program's code, without changing its functionality, 
is called "refactoring". It is an essential part of programming. Often, 
when you are first exploring an area, you will create code that works, 
but isn't well-organized for future maintainability.

In this case, we've created classes that pertain to the Monome-
Dinome cipher specifically, but we've also created classes that might be useful 
with other ciphers. If we separate these into two files, we can use 
the more general classes with other ciphers without dragging the code for the Monome-
Dinome ciphers along. This isn't primarily an issue of efficiency in the 
execution of the code — our programs aren't going to take noticably longer 
because we're loading a couple of classes we don't use. 
Tt's a matter of maintainability. Separating classes out into files that share 
a related purpose makes it easier to find the classes that we need to work 
with. Changes that we've made are more easily tracked and identified. 
The system as a whole is easier to visualize and understand.

What we need to do, then, is to create a new file, 
monodi.py, and move to it the classes MonomeDinome and MdCrack. The 
remaining classes in crypto.py are unchanged.

    #!/usr/bin/env python
    import sys
    from operator import itemgetter

    class Text:
        def __init__(self, filename = 0):
            if filename:
                self.load(filename)

        def load(self, filename):
            fp = open(filename, "r")
            self.rawtext = fp.read()
            fp.close
            self.text = self.convert(self.rawtext)

        def set(self, lst):
            self.text = "".join(lst)

        def get(self):
            return self.text

        def convert(self, txt):
            rval = ""
            for c in txt.upper():
                if c.isalpha():
                    rval += c
            return rval

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

        def get(self):
            return self.count

        def __str__(self):
            items = self.count.items()
            items.sort(key=itemgetter(1), reverse=True)
            rval = []
            for i in items:
                rval.append(i.__str__())
             
            return "\n".join(rval)
        
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
            

The new file, monodi.py, will need the same import statements that 
crypto.py had, and it will also need to import crypto.py 
itself. We're using the alternative form of the import statement, so 
that the contents of crypto.py are loaded into the monodi namespace. This 
allows us to access the Text class in the interpreter with monodi.Text. 
If we had used the first form, we'd have had to access as monodi.crypto.Text

    #!/usr/bin/env python
    import sys
    from operator import itemgetter
    from crypto import *

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
        
        def findLowestLetter(self, lst):
            pos = -1
            lowest = ''
            
            for i in range(len(lst)):
                if lst[i].isalpha():
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

            ct =Text()
            ct.set(rlist)
            return ct

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

            return rval

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
            
Creating the program framework
------------------------------

With our code refactored in this way, we now have a file that contains 
only code concerning the Monome-Dinome cipher. This file is as good a 
place as any to put our Monome-Dinome application code. We can do 
this using the __name__ idiom we explained before. The difference is that we'
ll want to be able to set options on the command line.

We handle command-line arguments with Python's OptionParser module. We add 
the options, parse the options, validate that a consistent set of options were 
set, then call a do-nothing function that will be replaced to do 
the work.

    def main():

        parser = OptionParser()
        parser.add_option("-e", "--encrypt", action="store_true", dest="encrypt")
        parser.add_option("-d", "--decrypt", action="store_true", dest="decrypt")
        parser.add_option("-b", "--break", action="store_true", dest="doBreak")
        parser.add_option("-l", "--letter_keyword", dest="letterKeyword")
        parser.add_option("-n", "--number_keyword", dest="numberKeyword")
        parser.add_option("-o", "--output", dest="outputFilename")

        (options, args) = parser.parse_args()

        if (options.encrypt and options.decrypt or 
        options.encrypt and options.doBreak or 
        options.decrypt and options.doBreak):
            print "Encrypt, decrypt and break are mutually exclusive"
            sys.exit(1)
               
        if not options.encrypt and not options.decrypt and not options.doBreak:
            options.encrypt = True # encrypt is default

        if ((options.encrypt or options.decrypt) and
                (not options.letterKeyword or not options.numberKeyword)):
            print "Must set keywords if encrypt or decrypt"
            sys.exit(1)

        if (options.doBreak and
                (options.letterKeyword or options.numberKeyword)):
            print "Cannot set keywords if break"
            sys.exit(1)

        if options.encrypt:
            encrypt(options.letterKeyword, options.numberKeyword, options.output, args)
        elif options.decrypt:
            decrypt(options.letterKeyword, options.numberKeyword, options.output, args)
        elif options.doBreak:
            doBreak(options.outputFilename, args)

    def encrypt(letterKeyword, numberKeyword, outputFilename, args):
        print "encrypt"

    def decrypt(letterKeyword, numberKeyword, outputFilename, args):
        print "decrypt"

    def doBreak(outputFilename, args):
        print "break"


    if __name__ == "__main__":
        main()

Encrypting
----------

We've done the encrypting many times, already. The only difference here 
is that we want to encrypt the contents of every filename passed on the command
-line, or read from standard-input if no filenames were passed, 
and we will want to write the results to the output file if one was 
specified.

The implementation is fairly simple. We read standard-input or the content of 
a file into a string, encrypt the string, and then write the result 
to the output. We discover, in the process, that we no longer 
have a method for loading a crypto,Text object from a string, only 
from a filename, so we add a method to do this.

    class Text:
        [...]
        def loadString(self, textString):
            self.rawtext = textString
            self.text = self.convert(textString)

With that in place, the encryption function is simple enough. Instead of creating 
a special case to handle when there are no filenames passed on the command-
line, we add the special filename "-", which we then take to mean read 
from standard input. This means that we can insert standard input into our list 
of files at any point. I can't see any partticular reason why 
we would want to do this, but it's common usage on Unix 
systems, and it makes processing the file loop simple,

    def encrypt(letterKeyword, numberKeyword, outputFilename, args):
        md = MonomeDinome(letterKeyword, numberKeyword)
        pt = Text()
        
        if len(args) == 0:
            args.append("-")

        if outputFilename:
            outFile = open(outputFilename, "w")
        else:
            outFile = sys.stdout
            
        for arg in args:
            if arg == "-":
                text = sys.stdin.read()
            else:
                inFile = open(arg, "r")
                text = inFile.read()
                inFile.close()
            
            pt.loadString(text)
            ct = md.encrypt(pt)
            outFile.write(str(ct))

        outFile.write("\n")
        
        if outputFilename:
            outFile.close()

Now we can encypt files on the command-line.

    bash 3.2$ monodi.py -h
    usage: monodi.py [options]

    options:
      -h, --help            show this help message and exit
      -e, --encrypt         
      -d, --decrypt         
      -b, --break           
      -l LETTERKEYWORD, --letter_keyword=LETTERKEYWORD
      -n NUMBERKEYWORD, --number_keyword=NUMBERKEYWORD
      -o OUTPUTFILENAME, --output=OUTPUTFILENAME
    bash 3.2$ monodi.py -e -nfred -lwilma plaintext.txt 
    61313 48484 76131 60131 06310 05493 48936 46464 34791 60248 15475 10484
    81531 31534 93431 30104 26104 31016 61347 10613 06131 81048 90104 93489
    36461 33439 42106 13160 42651 04248 15942 10507 15994 34794 26510 48705
    43138 48160 30543 86473 04604 67031 04200 17681 04347 13106 47101 64204
    74330 64543 19051 04261 06456 39049 63104 82461 84543 20234 84610 42047
    48471 66139 48434 79104 87013 49047 10434 72434 75434 79464 31310 64001
    34347 46184 81647 49348 93646 13
    bash 3.2$ monodi.py -e -nfred -lwilma plaintext.txt -o ciphertext.txt 
    bash 3.2$ 
            
Decrypting
----------

Decrypting should be a simple reversal of encrypting, Writing the function should be almost 
mechanical.

    def decrypt(letterKeyword, numberKeyword, outputFilename, args):
        md = MonomeDinome(letterKeyword, numberKeyword)
        ct = Text()
        
        if len(args) == 0:
            args.append("-")

        if outputFilename:
            outFile = open(outputFilename, "w")
        else:
            outFile = sys.stdout
            
        for arg in args:
            if arg == "-":
                text = sys.stdin.read()
            else:
                inFile = open(arg, "r")
                text = inFile.read()
                inFile.close()
            
            ct.loadString(text)
            pt = md.decrypt(ct)
            outFile.write(str(pt))

        outFile.write("\n")
        
        if outputFilename:
            outFile.close()

This seems straightforward enough, but it doesn't work. The problem isn't 
in this function, it's in the crypto.Text class.
Its convert() method, strips out all non-alphabetic characters. Our 
ciphertext contains nothing but non-alphabetic characters. We're trying to use 
the same class to represent two very different types of texts. So what do we do?

One option is to generalize the class — allow it to contain either letters or 
digits. This is a simple change, just replace isalpha() with isalnum(). 
This, though, would mean we wouldn't be filtering digits out of 
texts that should not have digits and letters from texts that should not 
have letters.

Another option is to create a separate class for each type of plaintext. That 
would allow each to filter for just the characters it needs. This seems like 
a bit of overkill, though, when the only difference between them would be 
the filtering function.

The third choice is to parameterize the filter function. That is, to make 
the filter function that we use a parameter that we pass when constructing the object
. This is easily done.

    class Text:
        [...]
        def __init__(self, filterFunc, filename = 0):
            self.filterFunc = filterFunc
            if filename:
                self.load(filename)

        def convert(self, txt):
            rval = ""
            for c in txt.upper():
                if self.filterFunc(c):
                    rval += c
            return rval

Note that we're still converting to upper-case. We may well
, one day, work with a cipher that handles mixed-case text. 
If we do, we will need to revisit our Text class. For now
, we won't worry about it.

The new parameter means that we must pass a filter function when we construct a 
Text object. These functions are simple, taking a single character as an argument 
and returning True or False. The simplest way of implementing these is to add 
the functions to the MonomeDinome module.

    def isalpha(c):
        return c.isalpha()

    def isdigit(c):
        return c.isdigit()

Everyplace we create a Text object, we need to pass a filter function.

    def decrypt(letterKeyword, numberKeyword, outputFilename, args):
        md = MonomeDinome(letterKeyword, numberKeyword)
        ct = Text(isdigit)
        [...]    

And now it works.

    bash 3.2$ monodi.py -d -nfred -lwilma ciphertext.txt 
    ASSOO NASVE START EDPRO GRAMM INGVE FOUND TOOUR SURPR ISETH ATITV ASNTA
    SEASY TOGET PROGR AMSRI GHTAS VEHAD THOUG HTDEB UGGIN GHADT OBEDI SCOVE
    REDIC ANREM EMBER THEEX ACTIN STANT VHENI REALI ZEDTH ATALA RGEPA RTOFM
    YLIFE FROMT HENON VASGO INGTO BESPE NTINF INDIN GMIST AKESI NMYOV NPROG
    RAMS
    bash 3.2$ 
            
Breaking
--------

Writing the breaking function, finally, is straightforward, with no surprises.

    def doBreak(outputFilename, args):
        ct = Text(isdigit)
        
        if len(args) == 0:
            args.append("-")

        if outputFilename:
            outFile = open(outputFilename, "w")
        else:
            outFile = sys.stdout
            
        for arg in args:
            if arg == "-":
                text = sys.stdin.read()
            else:
                inFile = open(arg, "r")
                text = inFile.read()
                inFile.close()

            ct.loadString(text)
            mdc = MdCrack(ct)
            md = MonomeDinome(mdc.ic()[0])
            pt = md.decrypt(ct)
            outFile.write(str(pt))
            outFile.write("\n")
        
        if outputFilename:
            outFile.close()

The program runs exactly as we expect, producing a simple substitition of the original 
plaintext. The doubled letters make this easy to recognize.

    bash 3.2$ monodi.py -b ciphertext.txt 
    ELLYY XELNA LIECI ADZCY HCEVV TXHNA BYMXD IYYMC LMCZC TLAIS EITIN ELXIE
    LAELP IYHAI ZCYHC EVLCT HSIEL NASED ISYMH SIDAF MHHTX HSEDI YFADT LGYNA
    CADTG EXCAV AVFAC ISAAO EGITX LIEXI NSAXT CAEUT QADIS EIEUE CHAZE CIYBV
    PUTBA BCYVI SAXYX NELHY TXHIY FALZA XITXB TXDTX HVTLI ERALT XVPYN XZCYH
    CEVL
    bash 3.2$ 

Passed through Decrypto 8.5 produces an almost perfect plaintext.

    AS SOON AS WE STARTED PROGRAMMING WE FOUND TO OUR SURPRISE THAT IT WASN T
    A SEASY TOGET PROGRAMS RIGHT AS WE HAD THOUGHT DEBUGGING HAD TO BE
    DISCOWEREDICAN REMEMBER THE EXACT INSTANT WHEN I REALIVED THAT ALARGE
    PART OF MY LIFE FROM THEN ON WAS GOING TO BE SPENT IN FINDING MISTAKES
    IN MY OWN PROGRAMS

