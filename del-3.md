# Idiomatisk Python - del 3 av 3

TODO: introduksjon.

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