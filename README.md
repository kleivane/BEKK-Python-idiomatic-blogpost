
# Idiomatisk Python

TODO: introduksjon.

Bloggposten er basert på [del 2](http://magnhaug.github.com/BEKK-Python-Kurs/slides/del2.html#1) av BEKKs [kursserie om Python](https://github.com/magnhaug/BEKK-Python-Kurs) på NTNU, og er derfor beregnet på lesere som kjenner grunnleggende Python uten å være dypt kjent med språket.

## Hva er Pythonisk kode?

Pythonisk kode er kode som bruker vanlige idiomer i Python på en god måte, i stedet for å implementere koden ved hjelp av konsepter vanligere i andre språk.

Dette kan illustreres med et enkelt eksempel hentet fra Pythons [offesielle ordliste](http://docs.python.org/glossary.html#term-pythonic).
I mange språk er det vanlig å iterere over elementer i lister ved hjelp av en eksplisitt indeks og en for-løkke.
Dette er også mulig i Python, og kan gjøres slik:

    for i in range(len(food)):
        print food[i]

I Python er det imidlertid et vanlig idiom å iterere over alle elementene i en sekvens direkte.
 
    for piece in food:
        print piece

Den beste beskrivelsen av hva idiomatisk Python virkelig dreier seg om er kanskje gitt i The Zen of Python:

    >> import this
    The Zen of Python, by Tim Peters

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!

Kort fortalt, kode skal være utrykksfull og lettlest.
Dette oppnår vi i Python ved å kjenne språket, og bruke de konstruktene som tilbys på riktig måte.
La oss dermed gå videre til å se på noen av de konseptene vi mener er viktige å kunne for å skrive god pythonisk kode.

## List comprehension

List comprehensions er en konsis syntaks for å lage eller transformere lister.
Ved hjelp av list comprehensions kan man enkelt iterere over eksisterende sekvenser, og transformere og filtrere elementene, og lagre resultatet som en liste.

Den viktigste fordelen med list comprehensions i forhold til å lage lister på vanlig måte ved hjelp av for-løkker er at koden blir kortere og mer lettlest, og dermed enklere å feilsøke.
Som en bonus kan også list comprehensions ha bedre ytelse enn vanlige for-løkker i mange tilfeller.

Syntaksen for å skrive list comprehensions er som følger.

    resultat = [uttrykk for element in liste if betingelse]

List comprehension er avgrenset av firkantparenteser, og resultatet lagres her i `resultat`.
`uttrykk` er et utrykk som evalueres for hver iterasjon av `for element in liste`, og resultatet av dette havner som et element i `resultat`. 
I de fleste tilfeller vil `uttrykk` være en operasjon over `element`.
Det siste leddet, `if betingelse` er valgfritt, og fungerer som et filter som lar oss ekskludere elementer vi ikke ønsker.

### Et enkelt eksempel

La oss ta utgangspunkt i kodesnutten under.
Her itererer vi over de 100 første heltallene, og lagrer alle kvadrattall som er delelige på 7 i en ny liste.

    >>> resultat = []
    >>> for i in range(100):
    ...     if i**2%7 == 0:
    ...         resultat.append(i**2)
    ... 
    >>> resultat
    [0, 49, 196, 441, 784, 1225, 1764, 2401, 3136, 3969, 4900, 5929, 7056, 8281, 9604]

Denne koden kan gjøres mye penere ved hjelp av list comprehensions:
r
    >> [i**2 for i in range(100) if i**7%5 == 0]
    [0, 49, 196, 441, 784, 1225, 1764, 2401, 3136, 3969, 4900, 5929, 7056, 8281, 9604]
    
Kodesnuttene over utrykker nøyaktig det samme, men den siste benytter et vanlig idiom i Python og blir dermed kortere og langt mer lettlest.

### Comprehensions med nøstede løkker

List comprehensions kan også gjøres med nestede for-løkker, ved å liste disse etter hverandre i uttrykket.

I eksempelet under lister vi opp alle permutasjoner av bokstavene x, y og z.

    >>> [a+b+c for a in "xyz" for b in "xyz" for c in "xyz"]
    ['xxx', 'xxy', 'xxz', 'xyx', 'xyy', 'xyz', 'xzx', 'xzy', 'xzz', 'yxx', 'yxy', 'yxz', 'yyx', 'yyy',
     'yyz', 'yzx', 'yzy', 'yzz', 'zxx', 'zxy', 'zxz', 'zyx', 'zyy', 'zyz', 'zzx', 'zzy', 'zzz']

I dette tilfellet kan vi altså eliminiere tre nestede for-løkker, og samtidig øke lesbarheten.
Vær imidlertid varsom med å ta dette for langt.
Håpløst komplekse list comprehensions som gjør for mye på en gang er gjerne vanskeligere å lese enn de tilsvarende nøstede for-løkkene, og strider dermed mot tankegangen i Zen og Python!

I eksempelet over kan det forøvrig bemerkes at vi itererer over bokstavene i en streng på samme måte som vi vanligvis itererer over elementer i en liste.
Dette er et godt eksempel på [duck typing](http://en.wikipedia.org/wiki/Duck_typing), et viktig prinsipp i Python.
Duck typing er tanken om at det det som betyr noe ikke er hvilken type noe har, men hva du kan gjøre med det--i dette tilfellet iterering.

### Nøstede list comprehensions

I tillegg til å lage list comprehensions med nøstede for-løkker kan vi også lage nøstede list comprehensions.
Dette gjør vi enkelt og greit ved å erstatte et uttrykk fra comprehension-uttryket med en ny list comprehension.

Eksempelet under viser hvordan vi kan bruke dette til å generere gangetabllen for tallene fra 1 til 10 som en 2-dimensjonal liste.

    >>> from pprint import pprint
    >>>
    >>> tabell = [[i*j for i in range(1,11)] for j in range(1,11)]
    >>> pprint(tabell)
    [[1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
     [2, 4, 6, 8, 10, 12, 14, 16, 18, 20],
     [3, 6, 9, 12, 15, 18, 21, 24, 27, 30],
     [4, 8, 12, 16, 20, 24, 28, 32, 36, 40],
     [5, 10, 15, 20, 25, 30, 35, 40, 45, 50],
     [6, 12, 18, 24, 30, 36, 42, 48, 54, 60],
     [7, 14, 21, 28, 35, 42, 49, 56, 63, 70],
     [8, 16, 24, 32, 40, 48, 56, 64, 72, 80],
     [9, 18, 27, 36, 45, 54, 63, 72, 81, 90],
     [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]]

### Comprehensions for Set og Dictionaries

Fra Python 2.7 finnes det også tilsvarende comprehensions for set og dictionaries.

For set comprehension er syntaksen helt tilsvarende list comprehension, med unntak av at firkantparentesene er byttet ut med krøllparenteser.

    >>> [i%3 for i in range(10)]
    [0, 1, 2, 0, 1, 2, 0, 1, 2, 0]
    >>> {i%3 for i in range(10)}
    set([0, 1, 2])

Dict comprehensions bruker også krøllparenteser, men her må vi oppgi et nøkkel/verdi par i stedet for et enkelt element.
Eksempelet under demonstrerer hvordan vi kan lage en dict som kobler *i* og den tilhørende toer-potensen *2*<sup>*i*</sup>.

    >>> {i: 2**i for i in range(10)}
    {0: 1, 1: 2, 2: 4, 3: 8, 4: 16, 5: 32, 6: 64, 7: 128, 8: 256, 9: 512}

I tillegg til disse finnes det også en liknende notasjon som bruker vanlige parenteser.
Men for å lære om denne må vi først se nærmere på iteratorer og generatorer.

## Iteratorer og generatorer

Iteratorer er kanskje ikke veldig spennende, men det er en viktig byggesten.
Flere av de python-elementene vi allerede kjenner kan fungere som iteratorer..
F.eks. lister:

    >>> items = [1, 4, 5]
    >>> it = iter(items)
    >>> it.next()
    1
    >>> it.next()
    4
    >>> it.next()
    5
    >>> it.next()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    StopIteration
    >>>

Syntaktisk sukker

    >>> items = [1, 4, 5]
    >>> for i in items:
    ...     print i,
    ...
    1 4 5

La oss implementere en iterator selv:

    class countdown(object):
        def __init__(self,start):
            self.count = start
        def __iter__(self):
            return self
        def next(self):
            if self.count <= 0:
                raise StopIteration
            r = self.count
            self.count -= 1
            return r

Og bruke den:

    >>> c = countdown(5)
    >>> for i in c:
    ...     print i,
    ...
    5 4 3 2 1

Dette var mye styr, kan det gjøres enklere? Yep, med generatorer:

    def countdown(i):
        while i > 0:
            yield i
            i -= 1

Resultat:

    >>> for i in countdown(5):
    ...     print i,
    ...
    5 4 3 2 1

### Hva er nytteverdien?

Uendelige lister, store datamengder:

    >>> from itertools import count
    >>> c = count()
    >>> c.next()
    0
    >>> c.next()
    1
    >>> c.next()
    2

Spørsmål: Hvordan holder man en uendelig liste i minne?!

# Generator expressions

List comprehensions kan automagisk skrives som en generator:

    >>> liste = [i for i in range(10)]
    >>> print liste
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    >>> for i in liste:
    ...   print i,
    ... 
    0 1 2 3 4 5 6 7 8 9
    >>> for i in liste:
    ...   print i,
    ... 
    0 1 2 3 4 5 6 7 8 9

&nbsp;

    >>> generator = (i for i in range(10))
    >>> print generator
    <generator object <genexpr> at 0x10ff3e410>
    >>> for i in generator:
    ...   print i,
    ... 
    0 1 2 3 4 5 6 7 8 9
    >>> for i in generator:
    ...   print i,
    ... 

Hva skjedde her?

### Fordeler

- Er mer kompakte (men kan være mindre fleksible) enn vanlige generatorer med yield.
- Er mer minnevennlige enn lister.
  - Uendelig lange lister passer dårlig i minnet...
- Som skapt for input til metoder som arbeider på elementer enkeltvis. (Da kan vi også droppe et sett parenteser.)

    
        sum(x for x in range(100000) if x%2 == 0)

### Ulemper

- Kan ikke itereres over flere ganger.
- Kan ikke bruke indeksering/slicing eller de vanlige liste-metodene (`append`, `insert`, `count`, `sort`, etc).


### Oppgaver:

1. Lag en list comprehension som lister opp alle partall under 20, og print dem
1. Lag et generator expression som gjør det samme. Print dem.
1. Lag en generator som lister opp ALLE partall.
1. Lag et generator expression som lister opp ALLE partall.
1. Print de første 20 av dem, f.eks. slik:  
    `print [alle_partall.next() for i in xrange(20)]`
1. Lag en generator som generer tall-sekvensen 1, -1, 2, -2, 3, -3, ...
1. Lag en generator som genererer fibonacci-tallene.
   Bruk denne til å finne det 100 000'ende fibonacci-tallet.

### Løsninger:

Lag en list comprehension som lister opp alle partall under 20, og print dem

    >>> partall = [i for i in xrange(20) if i%2==0]
    >>> for i in partall:
    ...   print i, 
    ... 
    0 2 4 6 8 10 12 14 16 18

Lag et generator expression som gjør det samme. Print dem.


    >>> partall = (i for i in xrange(20) if i%2==0)
    >>> for i in partall:
    ...   print i, 
    ... 
    0 2 4 6 8 10 12 14 16 18

Lag en generator som lister opp ALLE partall.

    >>> def allepartall():
    ...     i = 0
    ...     while True:
    ...             if i%2 == 0:
    ...                     yield i
    ...             i += 1

Lag et generator expression som lister opp ALLE partall.

    >>> from itertools import count
    >>> allepartall = (i for i in count() if i%2 == 0)

Lag en generator som generer tall-sekvensen 1, -1, 2, -2, 3, -3, ...

    >>> def plusminus():
    ...     i = 1
    ...     while True:
    ...             yield i
    ...             yield -i
    ...             i += 1

Lag en generator som genererer fibonacci-tallene.  
Bruk denne til å finne det 100 000'ende fibonacci-tallet.

    >>> def fib():
    ...     a, b = 1, 1
    ...     yield a
    ...     while True:
    ...             yield a
    ...             a, b = a+b, a
    >>> a = fib()
    >>> for i in xrange(100000):
    ...     result  = a.next()


## Funksjoner

Funksjoner i python er *førsteklasses*, og vi kan derfor gjøre mer med dem enn vi er vant med fra Java.
At funksjonene er førsteklasses vil si at vi kan...

1. sende dem inn som argumenter til andre funksjoner
1. returnere dem fra funksjoner
1. tilordne dem til variabler
1. lagre dem i datastrukturer

Vi har også større frihet i hvor vi kan definere funksjoner -- for eksempel inne i andre funksjoner.

Eksempel på hva man kan gjøre med funksjoner i Python

    >>> def n_doble(n): 
    ...     def fn(a):         # Definerer funksjon inne i en annen funksjon
    ...         return n*a
    ...     return fn          # Returnerer en funksjon
    ... 
    >>> dobling_funksjoner = {
    ... "to": n_doble(2),      # Lagrer funksjoner i en dictionary
    ... "tre": n_doble(3),
    ... "fire": n_doble(4),
    ... "hundre": n_doble(100)
    ... }
    >>> 
    >>> firedoble = dobling_funksjoner["fire"] # Tilordner funksjonen til en variabel
    >>> 
    >>> def kall(fn, *args):   # Tar inn en funksjon som parameter
    ...     return fn(*args)
    ... 
    >>> kall(firedoble, 3)     # Sender funksjon som argument
    12


## Lambda-funksjoner

Lambdaer er anonyme én-linjes funksjoner som består av en (valgfri) liste med parametere og ett uttrykk som evalueres og returneres.

Syntax: 


    lambda args: uttrykk

Vanlig funksjon:


    >>> def funksjonen_min(num):
    ...     return num * 2
    ...
    >>> funksjonen_min(3)
    6

Lambda:


    >>> lambdaen_min = lambda num: num * 2
    >>> lambdaen_min(3)
    6

Som alle andre funksjoner kan også lambaer brukes som argumenter.

    >>> def double( num ):
    ...     return num * 2
    ... 
    >>> map(double, range(10))
    [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

Dette blir ofte mer konsist med en lambda:

    >>> map(lambda num : num * 2, range(10))
    [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

Akkurat dette tilfellet blir kanskje vel så pent med en list comprehension:

    >>> [num * 2 for num in range(10)]
    [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

### Oppgaver

1. Lag en funksjon som regner ut gjennomsnittet av to tall, som den får som parametre.
1. Gjør om denne til en lambda.
1. Utvid lambda-funksjonen til å regne ut gjennomsnittet av en liste med tall.
1. Lag en funksjon make_adder(x), som returnerer en funksjon.  
  Den returnerte funksjonen skal legge til x til et tall.
1. Skriv kode som bruker make_adder.

### Løsninger

Lag en funksjon som regner ut gjennomsnittet av to tall, som den får som parametre.

    def avg(a, b):
        return (a+b)/2

Gjør om denne til en lambda.

    avg = lambda a, b: (a+b)/2

Utvid lambda-funksjonen til å regne ut gjennomsnittet av en liste med tall.

    avg = lambda alle_tall : sum(alle_tall) / len(alle_tall)

Lag en funksjon make_adder(x), som returnerer en funksjon.  
  Den returnerte funksjonen skal legge til x til et tall.

    >>> def make_adder(x):
    ...     def fn(y):
    ...             return y+x
    ...     return fn

Skriv kode som bruker make_adder.

    >>> add_two = make_adder(2)
    >>> add_two(40)
    42

## Decorators

Dekoratorer lar oss endre funksjonaliteten av eksisterende funksjoner uten å måtte gjøre endringer i selve funksjonene.

Syntaksen likner veldig på annotasjoner i Java:

    @dekorator
    def funksjon():
        return "foo"

Eksempelet på forrige slide er egentlig bare syntaktisk sukker for følgende:

    def funksjon():
        return "foo"
    funksjon = dekorator(funksjon)
    
En dekorator er altså en funksjon* som tar en funksjon som argument og returnerer en ny funksjon som fungerer som proxy/wrapper for den dekorerte funksjonen.

\* klasser kan også brukes, men vi holder oss til funksjoner her for å gjøre det enklere...

En typisk dekorator ser omtrent slik ut:

    def dekorator(fn):
        def ny_fn():
            # Her kan vi gjøre hva vi vil!
            # Vanligvis inkluderer det et kall til fn().
        return ny_fn

I den nye funksjonen kan vi definere funksjonalitet som skal skje før og etter den dekorerte funksjonen kalles. Eller vi kan la være å kalle den i det hele tatt. Eller vi kan endre argumentene den får inn. Eller...

### Et enkelt eksempel 

    >>> def foo(fn):
    ...     print "inne i dekoratoren"
    ...     def ny_fn():
    ...         print "starter wrapper-funksjonen"
    ...         fn()
    ...         print "slutter wrapper-funksjonen"
    ...     return ny_fn
    ... 
    >>> @foo
    ... def bar():
    ...     print "i den dekorerte funksjon"
    ... 
    inne i dekoratoren
    >>> bar()
    starter wrapper-funksjonen
    i den dekorerte funksjon
    slutter wrapper-funksjonen

For å dekorere funksjoner med ulikt antall argumenter bruker vi * og **.

### Eksempel: Dekorator som teller antall argumenter en funksjon får inn:

    >>> def count_args(fn):
    ...     def wrapper_fn(*args, **kwargs):
    ...         antall = len(args) + len(kwargs)
    ...         print "fikk inn %d argumenter" % antall
    ...         return fn(*args, **kwargs)
    ...     return wrapper_fn
    ... 
    >>> @count_args
    ... def foo(*args, **kwargs):
    ...     pass
    ... 
    >>> foo()
    fikk inn 0 argumenter
    >>> foo(1, 2, 3)
    fikk inn 3 argumenter
    >>> foo(1, 2, 3, bar="baz")
    fikk inn 4 argumenter
    >>> foo(*range(1000000))
    fikk inn 1000000 argumenter

Det fungerer også fint å stacke dekoratorer på hverandre.

    @dekorator1
    @dekorator2
    def funksjon():
        pass
    
Blir det samme som:

    def funksjon():
        pass
    funksjon = dekorator1(dekorator2(funksjon))

### Wraps

Dekoratorene vi har skrevet til nå erstatter funksjonene våre med nye funksjoner.
Vi har dermed endret på hvordan den dekorerte funksjonen ser ut utenfra, ved å endre navn, doc-string, etc.

    >>> def buu_dekorator(fn):
    ...     def ny_fn(*args, **kwargs):
    ...         # noe artig her
    ...         return fn(*args, **kwargs)
    ...     return ny_fn
    ... 
    >>> @buu_dekorator
    ... def foo():
    ...     """foo sin docstring"""
    ...     pass
    ... 
    >>> foo.__name__
    ny_fn
    >>> foo.__doc__
    None
        
Dette kan vi passende nok løse ved hjelp av enda en dekorator!

    >>> from functools import wraps
    >>> 
    >>> def yay_dekorator(fn):
    ...     @wraps(fn)
    ...     def ny_fn(*args, **kwargs):
    ...         # noe artig her
    ...         return fn(*args, **kwargs)
    ...     return ny_fn
    ... 
    >>> @yay_dekorator
    ... def foo():
    ...     """foo sin docstring"""
    ...     pass
    ... 
    >>> foo.__name__
    foo
    >>> foo.__doc__
    foo sin docstring

Vi kan også lage dekoratorer som tar inn parametere.

    >>> def gjenta(ganger):
    ...     def generert_dekorator(fn):
    ...         def wrapper(*args, **kwargs):
    ...             return [fn(*args, **kwargs) for i in range(ganger)]
    ...         return wrapper
    ...     return generert_dekorator
    ... 
    >>> @gjenta(4)
    ... def spam():
    ...     return "spam"
    ... 
    >>> spam()
    ['spam', 'spam', 'spam', 'spam']

Her er `gjenta` egentlig en funksjon som genererer dekoratorer. `gjenta(4)` lager dekoratoren som gjentar funksjonen 4 ganger, som brukes til å dekorere `spam`. Tilsvarende uten det syntaktiske sukkeret blir:

    >>> def spam():
    ...     return "spam"
    ... 
    >>> spam = gjenta(4)(spam)
    >>> spam()
    ['spam', 'spam', 'spam', 'spam']

### Oppgaver

1. Lag en `@ignore` decorator som gjør at kall til den dekorerte funksjonen ikke lenger gjør noenting.
1. Lag en dekorator `@timed` som tar tiden på den dekorerte funksjonen. 
1. Lag dekoratoren `@deprecated` som skriver ut en advarsel hvis noen bruker den dekorerte funksjonern. Bonus hvis du klarer å inkludere navnet på funksjonen i advarselen.
1. Gitt implementasjonen av fibonacci under, lag dekoratoren `@memoize` som lagrer og gjenbruker delløsningene. Regn så ut `fib(100)`.

Slik finner du tidspunkt i Python:


    from time import time
    tidspunkt = time()

Implementasjon av fibonacci:

    @memoize
    def fib(a):
        if a in (0,1): return a
        return fib(a-1) + fib(a-2)

### Løsninger

Lag en `@ignore` decorator som gjør at kall til den dekorerte funksjonen ikke lenger gjør noenting.

    def ignore(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            pass
        return wrapper


Lag en dekorator `@timed` som tar tiden på den dekorerte funksjonen. 

    from time import time

    def timed(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            start = time()
            resultat = fn(*args, **kwargs)
            print "brukte %f sekunder" % (time() - start)
            return resultat
        return wrapper

Lag dekoratoren `@deprecated` som skriver ut en advarsel hvis noen bruker den dekorerte funksjonern. Bonus hvis du klarer å inkludere navnet på funksjonen i advarselen.

    def deprecated(fn):
        @wraps(fn)
        def wrapper(*args, **kwargs):
            print "Warning: '%s' is deprecated!" % fn.__name__
            return fn(*args, **kwargs)
        return wrapper

Gitt implementasjonen av fibonacci under, lag dekoratoren `@memoize` som lagrer og gjenbruker delløsningene. Regn så ut `fib(100)`.

    def memoize(fn):
        cache = {}
        @wraps(fn)
        def wrapper(arg):
            if arg in cache:
                return cache[arg]
            else:
                cache[arg] = fn(arg)
                return cache[arg]
        return wrapper

## Oppsummering

- *List comprehensions* er en hendig syntaks for å lage/filtrere/mutere lister.
- *Generatorer* gir oss muligheten til å generere (uendelig) lange sekvenser uten at disse må lagres i minnet.
  - Generator-uttrykk er en kompakt og konsis syntaks for å lage generatorer.
- *Funksjoner* er første-klasses i Python, og kan derfor:
  - Brukes som argumenter og returverdier fra andre funksjoner.
  - Tilordnes variabler.
  - Lagres i datastrukturer.
- *Lambdaer* er enlinjes funksjoner uten navn.
- *Dekoratorer* lar oss endre funksjonalitet på eksisterende funksjoner uten å endre dem direkte.
  - Dekoratorene erstatter den dekorerte funksjonen med en ny funksjon.
  - I praksis bare hendig syntax for å drive med høyere ordens programmering.

---

Magnus Haug / Kjetil Valle
