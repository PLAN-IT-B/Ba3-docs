---
layout: default
title: Software
parent: De vuilbak
grand_parent: Puzzels
nav_order: 3
---

# Software

[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefGarbage/tree/main/Garbage)

- [Verschillende staten](#verschillende-staten)
- [Communicatie](#communicatie)
- [Overige functies](#overige-functies)

## Verschillende staten
Om de code overzichtelijk te maken hebben we gewerkt met verschillende staten. In welke staat de vuilbak zich bevind zal telkens in de loop bekeken worden aan de hand van booleans die veranderen. Deze booleans zijn:
* reset: Moet de ESP gereset worden?
* energie: Is er genoeg energie? Staat de buffer in de groene zone?
* actief: Is de code al eens juist ingegeven?
* checkVuilnisTotaal: Is al het vuilnis al gevonden?
* wachtOpGewicht: Is er al gewicht gedetecteerd?

Werken met deze staten was zeer handig aangezien er hierdoor geen probleem ontstond wanneer het niveau plots niet meer groen was. Het programma zou gewoon naar zijn "geenEnergie"-state gaan en alle variabelen die in het programma waren opgeslagen blijven ongewijzigd. Wanneer het niveau weer groen wordt kan er zonder problemen verder gedaan worden. 

Merk op dat er in vrijwel elke staat gebruikt gemaakt wordt van volgend stuk code:

```c++
if(bl ==false){
      lcd.backlight();
      bl = true;
    }
```
Dit stukje zorgt er voor dat de backlight van de LCD maar 1* wordt opgezet nandat deze door een energietekort uit gaat. Moesten we in de loop altijd lcd.backlight() oproepen krijgen we geflikker van het achtergrondlicht.

### resetPuzzel
Deze staat heeft de hoogste prioriteit. Wanneer het resetsignaal is ontvangen zal de ESP resetten.

### geenEnergie
Wanneer de buffer op rood of oranje staat zal het programma in deze staat komen. De backlight van de LCD zal uit blijven en het programma zal niet meer naar inputs luisteren (buiten de communicatie).

### enkelEnergie
In deze staat zal het programma enkel luisteren naar input van het toetsenbord. Het programma zal aan de hand van de de inputs van het toetsenbord een array vormen, eventuele typfouten kunnen gedelete worden door de '*'-toets. Wanneer de gebruiken vermoed de juiste code ingevuld te hebben kan deze op '#' drukken. Wanneer er dan 5 cijfers zijn ingegeven zal deze array vergeleken worden met de verkrege code-array van de wristbands en de train-game, als deze overeen komt zal de LCD "Code correct" afbeelden, zal er een succes-sound worden afgespeeld en zal de boolean actief op true worden gezet.

### puzzel
Dit is het eigenlijke deel van de puzzel. Deze staat zal beginnen met een simpele uitleg van de puzzel op de LCD te zetten. Hierna gaat hij kijken of een scanknop wordt ingedrukt. Wanneer dit het geval is zal de desbetreffende RFID scanner aangesproken worden, hierover later meer. Ten slotte zal het programma in deze staat checken of al het vuilnis al is gevonden, indien dit het geval is zal de boolean checkVuilnisTotaal op true gezet worden.

### gewichtWachter
Deze staat zal er voor zorgen dat het programma niet verder gaat totdat het afval na het scannen in de juiste vuilnisvak geworpen wordt. In de functie waar de RFID-scanner wordt opgeroepen zal indien een juist stuk afval gescand wordt een weegschaalnummer meegegeven worden. Op deze weegschaal wal een gewichtsverschil moeten wijn van minstens 10g voordat de puzzel weer verder kan gaan.

### eindePuzzel
Wanneer al het vuilnis in de vuilnisbak is zal deze staat activeren. Het totale gewicht van alle weegschalen zal slechts 1x genomen worden en zal zo op het LCD scherm worden geprint. De som van deze gewichten is de code die naar het UV-licht zal gestuurd worden. De puzzel is hier op zijn einde. Wanneer er niet genoeg energie is zal het scherm nog uit gaan maar op een reset na is er niets meer mogelijk om de eindcode en het eindbeeld te veranderen.

## Communicatie
Zoals besproken in communicatie verbinden we met de MQTT server. We subscriben in de code naar volgende mappen:
* wristbands/3cijfers (initieel zouden hier 3 cijfers in doorgestuurd worden, pas later werden dit er 4.)
* treingame/#
* controlpanel/reset
* TrappenMaar/zone

In de callback functie luisteren we naar verschillende boodschappen:
* Reset escaperoom: reset zal op true gezet worden en de ESP wordt gereset.
* groen, oranje of rood: Aan de hand hiervan zal de variabele "energie" gecontroleerd worden.
* Wristband-code .... : Wanneer "Wristband-code" in een bericht wordt ontvangen zal character 15-18 in de code array gezet worden. Dit komt overeen met de 4 cijfers die worden doorgestuurd.
*Trein-code .: Wanneer "Trein-code" in een bericht wordt ontvangen zal character 11 in de code array geplaatst worden.



## Overige functies

### scanRFID
Deze functie wordt opgeroepen wanneer het programma in de puzzel-state is en een scan knop ingedrukt wordt. Eerst wordt de multiplexer ingesteld om te sturen naar de correcte scanner, dan wordt er heb het scherm "Scanning ..." geschreven en daarna wordt er effectief gescand. De scanner zal een halve seconde zoeken achter een tag, wanneer er niets gescand wordt zal de functie beëindigd worden. 
Als er wel iets gescand wordt zal er gecontroleerd worden of het ID van de tag voorkomt in de rij van tags die bij deze scanner (soort vuilnis) horen. Als dit het geval is zal in de rij deze tag aangepast worden naar [0,0,0,0,0,0,0,0], hierdoor kan deze tag niet 2x gescand worden (In de gebruikelijke werking van de escape room maakt dit niet uit maar in een alternatieve versie waar je energie krijgt voor het correct sorteren wel). Ook zal er een succes geluid afgespeeld worden en zal het programma naar de gewichtWachter-staat gaan. 
Wanneer de tag fout is zal er een failure sound afgaan en zal er "kleine fout" naar de buffer gestuurd worden.

### Sound
Doordat we gebruik maken van het "Tone32" library konden we noten afspelen via een GPIO pin. Omdat de speaker niet zo luid ging en we controle wouden hebben over het volume hebben we er een versterker tussen geplaatst. 
