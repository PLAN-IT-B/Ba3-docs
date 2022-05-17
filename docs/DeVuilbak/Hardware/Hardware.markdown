---
layout: default
title: Hardware
parent: De vuilbak
grand_parent: Puzzels
nav_order: 1
---
# Hardware
## Inleiding
In deze sectie bespreken we de hardware-implementatie van de elektronische schakeling ter ondersteuning van de software die het gedrag van de escape room implementeert.
Het doel van deze schakeling is om enkele randaparaten te kunnen aansturen/benuttigen.
Deze randaparaten zijn:

- 3 RFID-modules
- 1 LCD-scherm
- 3 gewichtsensoren
- 1 toetsenbord
- 1 luidspreker
- 3 knoppen

Deze randaparaten willen we aansturen aan de hand van een  ESP32-WROOM-32E-ic.
Om deze randaparatuur naar behoeven te kunnen aansturen zullen er verder ook nog een I<sup>2</sup>C - multplexer en twee level shifters aanwezig moeten zijn op de PCB.
De level shifters moeten aanwezig zijn om 3 V I<sup>2</sup>C signalen om te zetten naar 5 V signalen.
De multiplexer is nodig omdat de 3 gebruikte RFID-scanner hetzelfde fixed I<sup>2</sup>C adres hebben.
Daarnaast willen we ook in staat zijn om de puzzel te voeden aan de hand van een powerbank (of dus met een 5 V - bron).
In de volgende secties vindt u de KiCad-schema's en andere bijkomende informatie voor het vervaardigen van de PCB.
Alsook vindt u hier de gebruikte randaperatuur voor het opstellen van de schema's.

## Specifieke randaparaten

[Hier](BOM.pdf) vindt u een gedetailleerde bestellijst (BOM) met daarin de componenten die in overweging genomen werden voor het ontwerpen van de PCB.
Daarnaast is ook in het bestand te vinden hoeveel van elke component gebruikt werd, en waar deze componenten te vinden zijn.

*Weerstanden en condensatoren werden niet opgenomen in de bill of materials.*

## Power circuit
![](Power_circuit.png)
## ESP32-WROOM-32E
![](esp_32.png)
## Multiplexer
![](Multiplexer.png)
## Randaparatuur
![](randaparatuur.png)
![](randaparatuur2.png)
## Level shifters
![](Level_shifters.png)
## Voledig schema

## Verbeteringen

## PCB
