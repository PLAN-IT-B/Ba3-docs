---
layout: default
title: Software
parent: De vuilbak
grand_parent: Puzzels
nav_order: 3
---
# Software
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
Dit is het eigenlijke deel van de puzzel. Deze staat zal beginnen met een simpele uitleg van de puzzel op de LCD te zetten

### gewichtWachter

### eindePuzzel





## Communicatie