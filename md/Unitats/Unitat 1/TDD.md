> Basat en el material publicat a [https://aulasoftwarelibre.github.io/taller-de-python/Testing/TDD/](https://aulasoftwarelibre.github.io/taller-de-python/Testing/TDD/) baix llicència Copyleft - CC BY-NC 4.0 - Aula de Software Libre

# Test Driven Developement

![TDD](https://marsner.com/wp-content/uploads/test-driven-development-TDD.png)

El desenvolupament guiat per proves de programari, o Test-driven development (TDD) és una pràctica d'enginyeria de programari que involucra dues pràctiques més: Escriure les proves primer (Test First Development) i Refactorització (Refactoring). Per escriure les proves generalment es fan servir les proves unitàries (unit test en anglès). En primer lloc, s'escriu una prova i es verifica que la nova prova falla. A continuació, s'implementa el codi que fa que la prova passe satisfactòriament i tot seguit es refactoritza el codi escrit. El propòsit del desenvolupament guiat per proves és aconseguir un codi net que funcione. La idea és que els requisits siguen traduïts a proves, així, quan les proves passen es garantirà que el programari compleix amb els requisits que s'han establert.

## Com podem implementar-lo en Python?

Si bé la biblioteca estàndard de Python ve amb *unittest* com a llibreria estàndar per a proves, Pytest és el framework de referència per provar el codi Python.

Pytest fa que siga fàcil escriure, organitzar i executar proves. En comparació amb unittest, de la biblioteca estàndard de Python, Pytest:

1. Requereix menys codi repetitiu perquè els seus conjunts de proves siguen més llegibles.
2. Suports de la plana *assertdeclaració*, que és molt més fàcil de llegir i més fàcil de recordar en comparació amb els *assertSomethingmètodes* - com *assertEquals*, *assertTruey* *assertContains*- en unittest.
3. S'actualitza més sovint, ja que no forma part de la biblioteca estàndard de Python.
4. Simplifica la configuració i el desmuntatge de l'estat de prova amb el sistema de fixació.
5. Utilitza un enfocament funcional.

A més, amb Pytest podem tindre un estil coherent a tots els nostres projectes de Python. Per exemple, suposem que teniu dues aplicacions web: una construïda amb Django i l'altra construïda amb Flask. Sense Pytest, el més probable és que aprofitem el *framework* de Django juntament amb una extensió de Flask com Flask-Testing. Per tant, els seus conjunts de proves tindrien estils diferents. Amb Pytest, d'altra banda, tots dos conjunts de proves tindrien un estil de codi coherent, cosa que facilitaria el salt de l'un a l'altre.

Pytest també té un gran ecosistema de complements mantingut per la comunitat.

Com per exemple:

- [Pytest-django](https://Pytest-django.readthedocs.io/): proporciona un conjunt de ferramentes creades específicament per a provar aplicacions de Django

- [Pytest-xdist](https://github.com/Pytest-dev/Pytest-xdist): s'utilitza per a executar proves en paral·lel

- [Pytest-cov](https://Pytest-cov.readthedocs.io/en/latest/): agrega soport de cobertura de codi

- [Pytest-instafail](https://github.com/Pytest-dev/Pytest-instafail): mostra fallos i errors inmediatament en lloc d'esperar fins el final d'una execució

## Pytest

![Pytest](https://warehouse-camo.ingress.cmh1.psfhosted.org/1599e7e4caeaac6ca1a8d4ace3cefa8a0d160925/68747470733a2f2f6769746875622e636f6d2f7079746573742d6465762f7079746573742f7261772f6d61696e2f646f632f656e2f696d672f7079746573745f6c6f676f5f6375727665732e737667)

El primer que hem de fer si volem utilitzar Pytest per testejar el nostre codi és instal·lar-lo. Per això obrirem una nova terminal i introduirem la següent ordre (fes-ho a un entorn virtual). Tal com es pot veure estem fent ús de pip el gestor de paquets de Python que prèviament havíem instal·lat.

    pip install pytest

Una vegada fet això podem comprovar la versió que tenim instal·lada escrivint la següent ordre:

    pytest --version

I obtindríem un resultat com aquest:

    This is pytest version 4.6.9, imported from /usr/lib/python2.7/dist-packages/pytest.pyc

Una vegada ho hem instal·lat i hem comprovat la versió que tenim arriba l'hora de començar a fer els nostres primers tests.

A l'hora d'escriure les proves cal que tant els fitxers on les escriurem com les mateixes funcions de prova dins del fitxer comencen amb el prefix *test_*, ja que si no, quan cridem a *pytest* passant-li la ruta del directori, aquest no les trobarà.

Un exemple de crida a Pytest seria una cosa així:

    pytest /home/ferran/Desktop/Curs

## Sintaxis

El funcionament de *pytest* és similar al de totes les llibreries de *testing*.

```python

assert Val1 == Val2   #¿Val1 igual Val2?
assert Val1 != Val2   #¿Val1 diferente Val2?
assert Val1 <  Val2   #¿Val1 menor que Val2?
assert Val1 <= Val2   #¿Val1 menor o igual que Val2?
assert Val1 >  Val2   #¿Val1 mayor que Val2?
assert Val1 >= Val2   #¿Val1 mayor o igual que Val2?
assert Val1 == TRUE   #¿Val1 igual TRUE?
assert Val1 == FALSE  #¿Val1 igual FALSE?

```

## Exemple

Suposem que hem d'implementar una calculadora. Per a això, definirem una sèrie de funcions que volem que estiguen disponibles a la nostra calculadora. Agafarem la funció *suma* per a continuar amb l'exemple. Si seguim al peu de la lletra les indicacions de TDD, el primer que hem de fer és crear la funció buida simplement per poder cridar-la, implementar la seva prova i executar-la comprovant la falla.

Per a això hem de crear un nou fitxer a qui anomenem *operations.py* en el que definirem les funcions i crearem un fitxer anomenat test_.py on crearem la nostra funció *test_operations*()


### Primers passos

#### operations.py
```python

def suma(sumad1, sumand2):
   return 0

```

#### test_.py
```python

import pytest
import operations

def test_operations():
   assert operations.suma(3,3) == 6

```

Una vegada hem fet això, procedim a la **refactorització**. Hem d'implementar la nostra funció encarregada de calcular la suma:

#### operations.py
```python

def suma(sumand1, sumand2):
   return sumand1 + sumand2
```
Una vegada implementada, hem d'executar les proves sobre la funció suma:

    pytest test_.py

I si tot ha anat bé, obtindrem un resultat similar a aquest:

    ===================================== comença la sessió de prova ========== ============================
    plataforma linux -- Python 3.8.5, pytest-6.2.3, py-1.10.0, pluggy-0.13.1
    rootdir: /home/ferran/Desktop/Curs
    collected 1 item                                                                              

    test_.py .                                                                              [100%]

    ====================================== 1 passat en 0,01 s ======= =================================

## Exercici

Acaba d'implementar les funcions resta, multiplicació, divisió, arrel, exponent i factorial i el·labora els seus testos corresponents.