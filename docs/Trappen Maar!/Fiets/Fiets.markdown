---
layout: default
title: Fiets
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 2
---
# Fiets
## Algemeen

Het spel valt en staat met een hometrainer waarvan de snelheid waaraan een speler fietst met een dynamometer wordt opmeten. De gemeten AC-spanning uit die dynamometer wordt met een bruggelijkrichter omgezet naar een gelijkspanning. Met behulp van een condensator wordt die gelijkgerichte spanning omgezet naar een
constante gelijkspanning welke via de functie analogRead(PinNummer) kan worden uitgelezen met de ESP32.
Door hier te kiezen voor een grote condensator kan het gefietste niveau makkelijker constant
gehouden worden aangezien de ontlaadtijd van deze condensator (τ = R · C) dan groter is.   

Met behulp van 3 blauwe leds die één voor één gaan branden, komt de fietser te weten hoeveel tijd
hij nog heeft om het correcte fietsniveau te bereiken. Via een RGB-led die groen of rood wordt, wordt getoond ofhet gefietste niveau correct was. De gefietste snelheid wordt visueel getoond a.d.h.v. een LCD (20x4) met i<sup>2</sup>C aansluiting.   

De DC spanning die ontstaat door het fietsen wordt tenslotte ook gebruikt om een UV-lamp aan te sturen die nodig is bij de laatste puzzel.

## KiCad project
Aangezien hier gebruik wordt gemaakt van een LCD die een voedingsspanning van 5V nodig heeft, is het het eenvoudigste om gebruik te maken van
een powerbank om de esp te voeden. Deze 5V wordt namelijk rechtstreeks als voeding gebruikt voor de LCD en voedt m.b.v. een LDO de ESP32 die 3.3V nodig heeft.

[Visit our Github to find our Kicad project!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/KiCad/Kicad-versie%20Bert/Fiets%20-%20zelfgemaakte%20level%20shifter/Fiets%20-%20zelfgemaakte%20level%20shifter)

### EEschema
![](2022-05-13-21-32-56.png)
### PCB ontwerp
![](2022-05-13-21-33-13.png)
![](2022-05-13-21-33-27.png)

## Code
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/MeasuringDcVoltageWithCommunicationBuffer)

## realisatie
FOTO TOEVOEGEN