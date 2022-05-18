---
layout: default
title: Hardware 
parent: De vuilbak
grand_parent: Puzzels
nav_order: 2
---
# Hardware

- [Inleiding](#inleiding)
- [Randapparaten](#randapparaten)
- [PCB](#pcb)
- [RFID](#rfid)
- [Toetsenbord](#toetsenbord)
- [LCD](#lcd)
- [Gewichtssensoren](#gewichtssensoren)


## Inleiding
In deze sectie bespreken we de hardware-implementatie van de elektronische schakeling ter ondersteuning van de software die het gedrag van de vuilbak implementeert.
Het doel van deze schakeling is om enkele randapparaten te kunnen aansturen/uitlezen.
Deze randapparaten zijn:

- 3 RFID-modules
- 1 LCD-scherm
- 3 gewichtssensoren
- 1 toetsenbord
- 1 luidspreker
- 3 knoppen

Deze randapparaten willen we aansturen aan de hand van een  ESP32-WROOM-32E-ic.
Om deze randapparatuur te kunnen aansturen zullen er verder ook nog een I<sup>2</sup>C - multiplexer en twee level shifters aanwezig moeten zijn op de PCB.
De level shifters moeten aanwezig zijn om 3.3 V I<sup>2</sup>C signalen om te zetten naar 5 V signalen.
De multiplexer is nodig omdat de 3 gebruikte RFID-scanner hetzelfde fixed I<sup>2</sup>C adres hebben.
Daarnaast willen we ook in staat zijn om de puzzel te voeden aan de hand van een powerbank (5V - bron).
In de volgende secties vindt u de KiCad-schema's en andere bijkomende informatie voor het vervaardigen van de PCB.
Alsook vindt u hier de gebruikte randapperatuur voor het opstellen van de schema's.

## Randapparaten

[Hier](BOM.pdf) vindt u een gedetailleerde bestellijst (BOM) met daarin de componenten die in overweging genomen werden voor het ontwerpen van de PCB.
Daarnaast is ook in het bestand te vinden hoeveel van elke component gebruikt werd, en waar deze componenten te vinden zijn.

*Weerstanden en condensatoren werden niet opgenomen in de bill of materials.*

## PCB
### KiCad-schema's
### Power circuit

![](Power_circuit.png)

#### ESP32-WROOM-32E
![](esp32.jpg)
#### Multiplexer

![](Multiplexer.png)

#### Randapparatuur

![](randapparatuur.png)
![](randapparatuur2.jpg)

#### Level shifters

![](Level_shifters.png)

#### Volledig schema
[Hier](Schema_VuilBak.pdf) vindt u het volledige schema terug in pdf-vorm.

### Verbeteringen

Na het daadwerkelijk vervaadigen van dit schema bleek dat hieraan nog heel wat verbeteringen nodig waren.
Zo zou het beter zijn geweest om enkele GPIO's een ander doel te geven dan deze waarvoor we ze aanvankelijk voorzien hadden. Zo zal het booten failen wanneer GPIO 12 hoog getrokken wordt, wat het geval is wanneer deze de gewichtsensor aanstuurt. We hebben deze dan gewisseld met GPIO23 die werd gebruikt voor de speaker. Ook konden we achteraf gezien GPIO's besparen door alle gewichtssensoren op dezelfde klok te hangen.

#### Randapperatuur
![](randaperatuur_verbetering.jpg)
![](Buttons_verbetering.jpg)
#### ESP32-WROOM-32E
![](esp32_verbetering.jpg)

### PCB
#### Front
![](PCB_front.jpg)
#### Back
![](PCB_back.jpg)

#### Opmerkingen

- Wij zien zelf in dat deze uitvoering van de PCB heel wat kleiner zou kunnen.
- Deze uivoering van de PCB is zonder de hierboven besproken verbeteringen.

## RFID

## Toetsenbord

## LCD

## Gewichtssensoren