# Metodologia CI/CD (integració i desplegaments continus)

![CI/CD](https://www.synopsys.com/content/dam/synopsys/sig-assets/images/cicd.svg.imgo.svg)

"La integració contínua és una pràctica de desenvolupament de programari on els membres d'un equip integren el seu treball amb freqüència, normalment cada persona integra almenys diàriament, donant lloc a múltiples integracions per dia. Cada integració es verifica mitjançant una compilació automatitzada (inclosa la prova) per detectar errors d'integració el més ràpidament possible".

La programació és iterativa, és a dir, es va repetint continuament. El codi font s'allotja a un repositori que és compartit per tots els membres de l'equip. Si voleu treballar en aquest producte, n'heu d'obtenir una còpia. Fareu canvis, els provareu i els tornareu a integrar al repositori principal.

No fa gaire, aquestes integracions eren grans i amb setmanes (o mesos) de diferència, causant mals de cap, perdre temps i perdre diners. Armats amb experiència, els desenvolupadors van començar a fer canvis menors i a integrar-los amb més freqüència. Això redueix les possibilitats d'introduir conflictes que cal resoldre més tard.

Després de cada integració, heu de crear el codi font. Construir significa transformar el vostre codi d'alt nivell en un format que el vostre ordinador sap executar. Finalment, el resultat es prova sistemàticament per assegurar-vos que als vostres canvis no han introduït errors.

## Perquè CD/CI?

A nivell d'equip, permet una millor cultura d'enginyeria, on proporcioneu valor aviat i sovint. Es fomenta la col·laboració i es detecten errors molt abans. La integració contínua permetrà:

- Fer que tu i el teu equip siguen més ràpids
- Donar-vos la confiança que esteu creant programari estable amb menys errors, i que el llançament a producció funciona
- Assegurar-vos que el vostre producte funcione en altres màquines, no només en el vostre ordinador portàtil
- Eliminar moltes despeses generals tedioses i permet centrar-vos en allò que importa
- Reduir el temps dedicat a resoldre conflictes (quan diferents persones modifiquen el mateix codi)

## Conceptes bàsics
Hi ha diverses idees i pràctiques clau que cal entendre per treballar de manera eficaç amb una integració contínua. A més, pot haver-hi algunes paraules i frases que no coneixeu, però que s'utilitzen sovint quan parleu de CI. Aquest capítol us introduirà en aquests conceptes i l'argot que els acompanya.

### Repositori d'origen únic
Si col·laboreu amb altres persones en una única base de codi, és habitual tenir un repositori compartit de codi font. Cada desenvolupador que treballa en el projecte crea una còpia local i fa canvis. Un cop estan satisfets amb els canvis, els fusionen de nou al repositori central.

S'ha convertit en un estàndard utilitzar sistemes de control de versions (VCS) com Git per gestionar aquest flux de treball. Els equips solen utilitzar un servei extern per allotjar el seu codi font i gestionar totes les parts mòbils. Els més populars són GitHub, BitBucket i GitLab.

Git us permet crear diverses branques d'un dipòsit. Cada branca és una còpia independent del codi font i es pot modificar sense afectar altres branques. Aquesta és una característica essencial, i la majoria dels equips tenen una branca principal (sovint anomenada branca mestra) que representa l'estat actual del projecte.

Si voleu afegir o modificar codi, hauríeu de crear una còpia de la branca principal i treballar a la vostra nova branca de desenvolupament. Un cop hàgiu acabat, torneu a combinar aquests canvis a la branca mestra.

![Branching](https://files.realpython.com/media/CI1-Branching_2.fe69572f8bcb.png)

El control de versions inclou més que codi. La documentació i els scripts de prova s'emmagatzemen normalment juntament amb el codi font. Alguns programes busquen fitxers externs utilitzats per configurar els seus paràmetres i paràmetres inicials. Altres aplicacions necessiten un esquema de base de dades. Tots aquests fitxers haurien d'anar al vostre repositori.

## Automatització de la construcció
Com s'ha esmentat anteriorment, construir el vostre codi significa agafar el codi font en brut, i tot el necessari per a la seva execució, i traduir-lo a un format que els ordinadors puguin executar directament. Python és un llenguatge interpretat, de manera que la seva "construcció" gira principalment al voltant de l'execució de proves en lloc de la compilació.

Executar aquests passos manualment després de cada petit canvi és tediós i requereix un temps i una atenció valuosos de la resolució de problemes real que esteu intentant fer. Una gran part de la integració contínua és automatitzar aquest procés.

Què significa això per a Python? Penseu en un fragment de codi més complicat que hàgiu escrit. Si heu utilitzat una biblioteca, paquet o *framework* que no ve amb la biblioteca estàndard de Python (penseu qualsevol cosa que necessiteu instal·lar amb pip), Python ha de saber-ho, de manera que el programa sàpiga on buscar quan trobe. ordres que no reconeix.

Emmagatzemeu una llista d'aquests paquets a requirements.txt o un fitxer Pipfile. Aquestes són les dependències del vostre codi i són necessàries per a una construcció exitosa.

Sovint escoltaràs la frase "trencant la construcció". Quan trenqueu la compilació, vol dir que heu introduït un canvi que va fer que el producte final no es puga utilitzar. No et preocupes. Li passa a tothom, fins i tot als desenvolupadors sèniors. Voleu evitar-ho principalment perquè impedirà que tots els altres treballen.

L'objectiu de CI és que tothom treballe en una base estable coneguda. Si clonen un repositori que està trencant la compilació, treballaran amb una versió trencada del codi i no podran introduir ni provar els seus canvis. Quan trenqueu la construcció, la màxima prioritat és arreglar-la perquè tothom puga reprendre la feina.

![Breaking change](https://files.realpython.com/media/CI1-BreakingChange.45a0e6b1fa07.png)

Quan la compilació/construcció del software està automatitzada, podeu integrar canvis amb freqüència, normalment diverses vegades al dia. Permet que les persones tinguen accés ràpidament als canvis i noten si hi ha un conflicte entre dos desenvolupadors. Si hi ha molts xicotets canvis en lloc d'unes quantes actualitzacions massives, és molt més fàcil localitzar on es va originar l'error. També us animarà a dividir el vostre treball en trossos més xicotets, cosa que és més fàcil de rastrejar i provar.

## Proves automatitzades
Com que tothom fa canvis diverses vegades al dia, és important saber que el vostre canvi no ha trencat res més al codi ni ha introduït errors. En moltes empreses, les proves són ara responsabilitat de tots els desenvolupadors. Si escriviu codi, hauríeu d'escriure proves. Com a mínim, hauríeu de cobrir totes les funcions noves amb una prova d'unitat.

L'execució de proves automàticament, amb cada canvi, és una bona manera de detectar errors. Una prova que falla automàticament fa que la compilació falle. Cridarà la vostra atenció sobre els problemes revelats per les proves, i la compilació fallida us farà corregir l'error que heu introduït. Les proves no garanteixen que el vostre codi estiga lliure d'errors, però protegeix de molts canvis descuidats.

Automatitzar l'execució de proves us proporciona més tranquil·litat perquè sabeu que el servidor provarà el vostre codi cada vegada que modifiqueu el codi contra el repositori central, fins i tot si us oblideu de fer-ho localment.

## Ús d'un servei d'integració contínua externa
Si alguna cosa funciona al vostre ordinador, funcionarà a tots els ordinadors? Probablement no. És una excusa clixé (el típic, "al meu ordinador va"). Fer que el codi funcione localment no és el final de la vostra responsabilitat.

Per fer front a aquest problema, la majoria de les empreses utilitzen un servei extern per gestionar la integració, de la mateixa manera que s'utilitza GitHub per allotjar el vostre repositori de codi font. Els serveis externs tenen servidors on creen codi i executen proves. Actuen com a monitors del vostre repositoris i impedeixen que ningú es fusione amb la branca mestra si els seus canvis trenquen la compilació.

![Merging with CI server](https://files.realpython.com/media/CI-Testing_1.dd4a5d09bedd.png)

Hi ha molts serveis d'aquest tipus, amb diferents característiques i preus. La majoria tenen un nivell gratuït perquè pugueu experimentar amb un dels vostres repositoris. Utilitzareu un servei anomenat en els exercis.

## Proves en un entorn preparat
Un entorn de producció és on finalment s'executarà el vostre programari. Fins i tot després de crear i provar la vostra aplicació amb èxit, no podeu estar segur que el vostre codi funcione a l'ordinador de destinació. És per això que els equips despleguen el producte final en un entorn que imite l'entorn de producció. Una vegada esteu segur que tot funciona, l'aplicació es desplega a l'entorn de producció.

Sentiràs que la gent parla d'aquest clon de l'entorn de producció utilitzant termes com ara entorn de desenvolupament o entorn de prova. És habitual utilitzar abreviatures com DEV per a l'entorn de desenvolupament i PROD per a l'entorn de producció.

L'entorn de desenvolupament ha de reproduir les condicions de producció el màxim possible. Aquesta configuració de vegades s'anomena paritat DEV/PROD. Mantingueu l'entorn de l'ordinador local el més semblant possible als entorns DEV i PROD per minimitzar les anomalies en desplegar aplicacions.

![local-dev-prod](https://files.realpython.com/media/DEV_PROD_2_2.ae771984fb55.png)

Estos conceptes que acabem de veure és el que s'anomena CD (continuous deployment).

## CI amb CircleCI i GitHub

CircleCI és un servei per a automatitzar integracions que es pot connectar a GitHub. Necessita saber com executar la vostra contrucció (build) i espera que aquesta informació es proporcione en un format determinat. Requereix una carpeta **.circleci** dins del vostre repositori i un fitxer de configuració dins. El fitxer de configuració conté instruccions per a tots els passos que el servidor de construcció ha d'executar. CircleCI espera que aquest fitxer es diga **config.yml**.

Un fitxer .yml utilitza un llenguatge de serialització de dades, YAML, i té la seva pròpia especificació. L'objectiu de YAML és ser llegible per l'home i funcionar bé amb llenguatges de programació moderns per a les tasques quotidianes comunes.

En un fitxer YAML, hi ha tres maneres bàsiques de representar dades:

- Mapes (parells clau-valor)
- Seqüències (llistes)
- Escalars (cadenes o números)

És molt senzill de llegir:
- El sagnat es pot utilitzar per a l'estructura.
- Els dos punts separen els parells clau-valor.
- Els guions s'utilitzen per crear llistes.

Este seria un exemple d'aquest arxiu d'integració:

```yaml
# Python CircleCI 2.0 configuration file
version: 2
jobs:
  build:
    docker:
      - image: circleci/python:3.7

    working_directory: ~/repo

    steps:
      # Step 1: obtain repo from GitHub
      - checkout
      # Step 2: create virtual env and install dependencies
      - run:
          name: install dependencies
          command: |
            python3 -m venv venv
            . venv/bin/activate
            pip install -r requirements.txt
      # Step 3: run linter and tests
      - run:
          name: run tests
          command: |
            . venv/bin/activate
            flake8 --exclude=venv* --statistics
            pytest -v --cov=calculator
```

### Repassem alguns conceptes:

Recordeu el problema que tenen els programadors quan alguna cosa funciona al seu ordinador portàtil però, funcionarà a qualsevol lloc? Abans, els desenvolupadors solien crear un programa que aïlla una part dels recursos físics de l'ordinador (memòria, disc dur, etc.) i els converteix en una màquina virtual.

Una màquina virtual pretén ser tot un ordinador per si mateix. Fins i tot tindrà el seu propi sistema operatiu. En aquest sistema operatiu, desplegueu la vostra aplicació o instal·leu la vostra biblioteca i la proveu.

Les màquines virtuals ocupen molts recursos, cosa que va provocar la invenció dels contenidors. La idea és anàloga als contenidors d'enviament. Abans que s'inventessin els contenidors d'enviament, els fabricants havien d'enviar mercaderies en una gran varietat de mides, embalatges i modes (camions, trens, vaixells).

Mitjançant l'estandardització del contenidor d'enviament, aquestes mercaderies es podrien transferir entre diferents mètodes d'enviament sense cap modificació. La mateixa idea s'aplica als contenidors de programari.

Els contenidors són una unitat lleugera de codi i les seves dependències en temps d'execució, empaquetats de manera estandarditzada, de manera que es poden connectar i executar ràpidament al sistema operatiu Linux. No cal que creeu un sistema operatiu virtual complet, com ho faríeu amb una màquina virtual.

Els contenidors només reprodueixen parts del sistema operatiu que necessiten per funcionar. Això redueix la seva mida i els dóna un gran augment de rendiment.

**Docker** és actualment la plataforma de contenidors líder i fins i tot és capaç d'executar contenidors Linux a Windows i macOS. Per crear un contenidor Docker, necessiteu una imatge de Docker. Les imatges proporcionen plànols per als contenidors de la mateixa manera que les classes proporcionen plànols per als objectes. Reprendrem el tema de Docker més endavant al llarg del nostre curs.

CircleCI manté imatges de Docker preconstruïdes per a diversos llenguatges de programació. Al fitxer de configuració anterior, heu especificat una imatge de Linux que ja té Python instal·lat. Aquesta imatge crearà un contenidor dins el qual es produirà la construcció del programari.

### Configuració de l'arxiu **config.yml**

Vegem cada línia del fitxer de configuració:

1. **version**: cada config.yml comença amb el número de versió de CircleCI, que s'utilitza per emetre advertències sobre canvis.
2. **jobs**: els treballs són els passos individuals que s'han de donar per a la construcció de l'aplicació. El *build* pot estar compost d'un sol *job*.
3. **build**: Com s'ha dit abans, *build* és el nom de la construcció.
4. **docker**: els passos d'un treball es produeixen en un entorn anomenat executor. L'executor comú de CircleCI és un contenidor Docker. És un entorn d'execució allotjat al núvol, però existeixen altres opcions, com ara un entorn macOS.
5. **imatge**: una imatge Docker és un fitxer utilitzat per crear un contenidor Docker en execució. Estem utilitzant una imatge que té Python 3.7 preinstal·lat.
6. **working_directory**: el vostre repositori s'ha de consultar en algun lloc del servidor de compilació. El directori de treball representa la ruta del fitxer on s'emmagatzemarà el repositori.
7. **steps**: aquesta clau marca l'inici d'una llista de passos que ha de realitzar el servidor de compilació.
8. **checkout**: el primer pas que ha de fer el servidor és obtindre el codi font i portar-lo al directori de treball. Això es realitza mitjançant un pas especial anomenat *checkout*.
9. **run**: l'execució de programes o ordres de línia d'ordres es fa dins de la clau *command*. Les ordres s'executaran dins la shell del contenidor.
10. **name**: la interfície d'usuari de CircleCI us mostra cada pas de construcció en forma de secció ampliable. El títol de la secció es pren del valor associat a la clau *name*.
11. **command**: aquesta clau representa l'ordre que s'executa mitjançant l'intèrpret d'ordres. El símbol | és el caràcter que separa cadascuna de les ordres, exactament com si executarem un script de shell/bash.

Per a saber més, consulta la següent [guia de referència de configuració de CircleCI](https://circleci.com/docs/2.0/configuration-reference/).

### Workflow

El nostre *workflow* és molt senzill i consta de 3 passos:

1. Consulta del repositori
2. Instal·lació de les dependències en un entorn virtual
3. Execució del linter i proves dins de l'entorn virtual
   
Ara tenim tot el que necessitem per iniciar el nostre *pipeline*. 

1. Inicieu sessió al vostre compte de CircleCI i feu clic a Afegeix projectes. 
2. Localitzeu el vostre repositori i feu clic a Configura el projecte. 
3. Seleccioneu Python com a llenguatge. Com que ja tenim un config.yml, podem saltar els passos següents i fer clic a Comença a crear.

CircleCI us portarà al tauler d'execució de la vostra feina. Si heu seguit tots els passos correctament, hauríeu de veure que la vostra feina ha passat els tests.

El repositori hauria de contindre ara els vostres canvis.

### Execrici 1

Configura el codi de calculadora per fer una integració automatitzada.

## Fem canvis al nostre codi i els integrem

Hi haurà dos tipus de canvis principalment:

1. Afegir nova funcionalitat. Aplicarem TDD a les noves funsions i afegirem el codi validat.
2. Refactoritzar. Comprovarem que les modificacions superen els nostres testos.

### Exercici 2

Refactoritza la funció factorial, implementant-la de forma recursiva i afegix la nova versió al codi.

### Exercici 3

Afegeix una nova funció de càlcul de potències a la calculadora. Fes la seua integració.