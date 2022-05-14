---
layout: default
title: Hardware
parent: De vuilbak
grand_parent: Puzzels
nav_order: 1
---
# Hardware
## Inleiding
In deze sectie bespreken we de hardwareimplementatie van de elektronische schakeling ter ondersteuning van de software die het gedrag van de escape room implementeert.
Het doel van deze schakeling is om enkele randaparaten te kunnen aansturen.
Deze randaparaten zijn:
- 3 RFID-modules
- 1 LCD - scherm
- 3 gewichtsensoren
- 1 toetsenbord
- 1 luidspreker <br /><br />
Deze randaparaten willen we aansturen aan de hand van een  ESP32-WROOM-32E ic.
Om deze randaparatuur naar behoeven te kunnen aansturen zullen er verder ook nog een I<sup>2</sup>C - multplexer en twee level shifters aanwezig moeten zijn.
De level shifters moeten aanwezig zijn om 3 V I<sup>2</sup>C signalen om te zetten naar 5 V signalen.
De multiplexer is nodig omdat de 3 gebruikte RFID-scanner hetzelfde fixed I<sup>2</sup>C adres hebben.
Daarnaast willen we ook in staat zijn om de puzzel te voeden aan de hand van een powerbank (of dus met een 5 V - bron).
In de volgende secties vindt u de KiCad-schema's voor het vervaardigen van de PCB.
Alsook de gebruikte randaperatuur voor het opstellen van de schema's
## specifieke randaparaten

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
![](randaparatuur2.png)
## Verbeteringen

## PCB
![](Power_circuit.png)