---
layout: default
title: Software
parent: Wristbands
grand_parent: Puzzels
nav_order: 1
---


# Software
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefWristbands/tree/main/Wristband-Code)

## Algemeen
De code bestaat uit 3 grote delen. Als eerste hebben we onze variabelen. Daarnaast hebben we al onze functie en dan ook nog de setup en de loop.

## Variabelen
### Unieke variabelen
Het eerste deel van de code bevat 2 belangerijke variabelen.
namelijk de naam van de esp en zijn ID. Die laatse wordt tijdens het verloop van het spel veranderd omdat deze wordt gebruikt voor de partnerruil. 

Elke wristband heeft zijn eigen espNaam en id. Het vreemd is dat ze als volgt worden gegeven:

    esp1 : ID = 3
    esp2 : ID = 4
    esp3 : ID = 1
    esp4 : ID = 2

Dit wordt zo gedaan zodat de eerste code die wordt verstuurd naar de vuilbakken niet "1234" is, maar "3412".  Dit wordt zo gedaan zodat het spel niet te makkelijk start.
    
```c
    int id = 3;
    std::string espNaam = "esp1";
```

### Aanpasbare variabelen
Vervolgens hebben we een deel variabelen die we kunnen aanpassen naargelang we bepaalde parameters in het spel willen veranderen.

```c
    int codetime =5000;
    int codetimelong =60000;
    const unsigned long switchPeriod = 600000;
    int partnerTime = 30;
    int grenswaardeTeVer  = 50;   
    int grenswaardeTeDicht = 50; 
    int irThreshold = 70000;
    int hartslagFoutenMax = 5;

    const unsigned long period = 1000;
    // #ms tussen het scannen van de andere ESP'S
```
* codetime: 
Tijdens het spel wordt een code duidelijk gemaakt door knipperlichtjes. Als de code 3412 is, dan gaat esp3 eerst een ledje laten knipperen, vervolgens knipperen respectievelijk esp4, esp1, esp2 ook een ledje. De tijd die hiervoor tussen het knipperen van twee opeenvolgende esp's is hier voorgesteld in milliseconden.

* codetimelong: De code die wordt beschreven in vorig puntje wordt in dit geval elke minuut opnieuw getoont.

* switchPeriod: Tijdens de duur van de escaperoom wordt er ook gedaan aan partnerruil. Deze variabele heeft aan na hoeveel milliseconden dit gebeurd. In dit geval duurt dit 10 minuten.

* patnerTime: In het begin van een nieuwe ronde van 10 minuten krijgen de spelers een aantal seconden tijd om hun partner te vinden. Tijdens deze periode worden geen fouten gestuurd naar de buffer.

* grenswaardeTeVer en grenswaardeTeDicht: Dit is een grenswaarde om te beslissen wanneer twee esp te dicht of te ver bij elkaar zijn. Deze waarde stelt de sterkte van het ontvangen BLE-signaal voor.

* hartslagFoutenMax: Wanneer er geen hartslag word gedetecteerd wordt er niet direct een foutmelding gestuurd naar de buffer. De fout moet zich namelijk 5 keer na elkaar voordoen vooralleer deze wordt gestuurd. Dit doen we om te voorkomen dat er bij een foute meting van de hartslagsensor direct een grote fout zou worden gestuurd.

* period: elke esp stuurt continu een signaal. Na een bepaalde periode van, in dit geval, één seconde zal de esp zijn omgeving scannen.