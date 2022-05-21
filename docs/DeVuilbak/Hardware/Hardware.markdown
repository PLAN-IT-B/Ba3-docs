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

## Breadboardimplementatie

## PCB
### KiCad-schema's
#### Power circuit

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
![](randapparatuur_verbetering.jpg)

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
De gebruikte RFID-module is de PN532 - NFC/RFID - Module.
De datasheet van deze component is [hier](PN532_C1.pdf) te vinden 

![](pn532.jpg)

Deze module's werkt via I<sup>2</sup>C communicatie. Wanneer deze sensor wordt aangesproken zal hij beginnen zoeken naar een tag. In de code zal ervoor gezorgd worden dat hij maar maximum een halve seconde zoekt naar een tag. Wanneer deze dan niets heeft gevonden zal het programma weer terug naar de gewone puzzelstaat gaan.

Het probleem bij het gebruiken van meerdere scanners op 1 I<sup>2</sup>C lijn is dat ze allemaal hetzelfde address hebben (0x48). Hierdoor hebben we een I<sup>2</sup>C-multiplexer nodig. Deze chip zal ervoor zorgen dat er slechts 1 module tegelijk wordt aangesproken. 
We gebruiken de TCA9548A op het ingestelde adres 0x70. Op kanaal 5, 6 en 7 sluiten we de scanners aan.

Samen met de VCC, ground en de I<sup>2</sup>C pinnen moeten ook normaal gezien de reset en IRQ pinnen aangesloten worden. Wij hebben besloten om alle IRQ pinnen op 1 pin aan te sluiten aangezien we maar 1 scanner tegelijk gebruiken. We hebben ondervonden dat de reset pin open mag blijven. 
Moesten we deze puzzel opnieuw ontwerpen waren we maar voor 1 scanner en scanplatform gegaan aangezien we er uiteindelijk voor hebben gekozen om niet automatisch te scannen maar knoppen te gebruiken. In dit geval zouden we de multiplexer ook niet nodig gehad hebben. 
De datasheet van de I<sup>2</sup>C-multiplexer is [hier](TCA.pdf) te vinden 

![](TCA.png)



## Toetsenbord
We gebruiken een 3x4 keypad met een output van 7 pinnen.
De datasheet van deze component is [hier](4x3_keypad.pdf) te vinden 
![](keypad.jpg)

## LCD
We gebruiken 20x4 LCD scherm via I<sup>2</sup>C. 
De datasheet van deze component is [hier](LCD.pdf) te vinden 

![](LCD.jpg)

## Gewichtssensoren
Als gewichtssensor hebben wij gekozen voor een Load cell die een maximaal gewicht van 5kg aan kan. 

![](load_cell_5kg.jpg)

Deze wordt aangestuurd door een signaalversterker.
Meer precies de HX711 Load Cell versterker.
De datasheet van deze component is [hier](hx711_english.pdf) te vinden.

![](HX711.jpg)

De signaalversterker verwacht een voedingspanning tussen 2.7-en 5.5V.
Wij hebben hier gekozen voor een voedingsspanning van 3.3V
Aan de module worden enkele pinnen naar buiten gebracht.
Deze pinnen hebben de volgende doeleinden:

- GND : Deze pin dient verbonden te worden aan de GND van onze PCB
- VCC: Hier moet een voedingsspanning van 2.7-5.5V aangeboden worden. 
- DT : Dit is de data-pin van de signaalversterker. 
- SCK: Dit is de pin waar een kloksignaal op aangeboden moet worden

De sensor is verbonden aan de amplifier op deze manier:

- Rood -> E+
- Zwart -> E-
- Wit -> A-
- Groen -> A+