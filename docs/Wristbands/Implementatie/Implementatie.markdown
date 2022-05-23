---
layout: default
title: Implementatie
parent: Wristbands
grand_parent: Puzzels
nav_order: 5
---

# Implementatie

> [Visit our Github to find our SolidWorks project and the STL-files!](https://github.com/PLAN-IT-B/BachelorProefWristbands/tree/main/3D-ontwerp)

**Overzicht:**

- [3D-model solidworks](#3d-model-solidworks)
- [prakstische realisatie](#praktische-realisatie)
- [De Ruimte](#de-ruimte)

## 3D-model Solidworks
Onze module wordt een klein doosje waar onze PCB en de batterij in terecht komen. Deze batterij wordt verbonden met de PCB via een switch. Deze hebben we achteraf pas toegevoegd wanneer de PCB reeds ontworpen was.

In het doosje bevinden zich een aantal openingen. We hebben enerzijds een opening in de bodem zodat de hartslagsensor de arm van de speler kan bereiken. Daarnaast zit er ook een ronde opening in de zijkant van het doosje. Dit is bedoeld zodat we de RX-pin van de ESP makkelijk zouden kunnen bereiken om de esp te programmeren. Bovendien wordt de verbindingskabel van de antenna via deze weg in onze module gebracht. Ten slotte bevat het doosje 4 gaatjes waar eventueel de bandjes door gemonteerd kunnen worden. 

Het dekseltje is bevat een opening waar men het ledje door kan zien. Dit deksel wordt gemonteerd op het doosje met ducktape. 

Hieronder vindt u een overzicht van het model.

|**Overzichtsmodel**|**Bovenaanzicht** |
|:----:|:----:|
|![](totaalplaatje.png)|![](boven_aanzicht.png)|
|[Overzichtsmodel in het groot](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefWristbands/main/3D-ontwerp/Afbeeldingen%20ontwerp/totaalplaatje.png)|[Bovenaantzicht in het groot](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefWristbands/main/3D-ontwerp/Afbeeldingen%20ontwerp/boven%20aanzicht.png)|
|[De Case 3D](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/3D-ontwerp/caseWristband.STL)|[De Top 3D](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/3D-ontwerp/topWristband.STLp)|


## Praktische realisatie

### De Wristbands
Vorig 3D-model hebben we 4 maal geprint met een 3D-printer, voor elke wristband. Vervolgens hebben we de antenna aan de zijkant van het doosje gelijmd. We gebruikten een ducktape met een leuke structuur om het deskel aan zijn doosje te monteren. Ten slotte lijmden we velcro aan de onderkant. We gebruikten de voorziene gaatjes niet omdat de wristband dan minder goed aansloot aan de arm. 

Voor de afwerking hebben we gaatje voor de led afgeplakt met wat doorzichtig plastic zodat het licht mooier zichtbaar wordt. Daarnaast hebben we met een witte paint marker de cijfers 1 tot en met 4 op de wristbands geschreven in de vorm van 7-segment displays. Het resultaat van dit alles vindt u hieronder:

|Wristband 1| De 4 Wristbands|
|:---:|:---:|
|![](Wristband2.jpg)|![](4Wristbands1.jpg)|
|[Wristband 1 groot](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/Wristband2.jpg?raw=true)|[De 4 Wristbands groot](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/4Wristbands1.jpg?raw=true)|

|De binnenkant|De onderkant|
|:---:|:---:|
|![](Binnenkant1.jpg)|![](Achterkant.jpg)|
|[De binnenkant groot](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/Binnenkant1.jpg?raw=true)|[De onderkant groot](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/Achterkant.jpg?raw=true)|

### De Ruimte

Aangezien onze modules afstanden bekijken ten opzicht van elkaar is het aangewezen om een ruimte te voorzien waar de spelers effectief afstand van elkaar kunnen houden. Wanneer men de escaperoom zou uitbereiden en een grotere kamer zou nodig hebben, kan men de variabelen *grensWaardeTeVer* en *grensWaardeTeDicht* in de code aanpassen tot men waarden komt die voldoende aangepast zijn aan ruimte.

### Extra blaadjes in de ruimte

Omdat de kleurencode van dit spel niet zo van zelfsprekend is, wordt er een legende opgehangen in de kamer. Deze ziet er uit als volgt: 

![](LEGENDE.png)

[Legende in github](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/LEGENDE.png?raw=true)


Daarnaast wordt er vermeld dat de wristbands in een bepaalde volgorde paars flikkeren. Dit ziet er uit als volgt:

![](TIP.png)
[Tip in github](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/Afbeeldingen/Afbeeldingen%20Realisatie/TIP.png?raw=true)