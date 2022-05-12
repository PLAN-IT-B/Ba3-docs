---
layout: default
title: Fiets
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 2
---
# Fiets
## Algemeen

Ons spel valt en staat met een hometrainer waarvan we met een dynamometer de snelheid dat de speler fiets
kunnen opmeten. De gemeten AC-spanning uit onze dynamometer zetten we met een Bruggelijkrichter om
naar een pulserende DC spanning. Met behulp van een condensator zetten we deze laatste dan om naar een
constante gelijkspanning welke we via de functie analogRead(PinNummer) uitlezen met de ESP32.
Door hier te kiezen voor een grote condensator zorgen we ervoor dat het gefietste niveau makkelijker constant
gehouden kan worden aangezien de ontlaadtijd van deze condensator τ = R · C dan groter is.   

Met behulp van 3 blauwe leds die  ́e ́en voor  ́e ́en gaan branden, laten we de gebruiker weten hoeveel tijd
hij nog heeft om het correcte fietsniveau te bereiken. Via een RGB-led die we of groen of rood laten worden
tonen we of het gefietste niveau correct was. De gefietste snelheid wordt visueel getoond a.d.h.v. een LCD
(20x4) met i<sup>2</sup>C aansluiting.   

De DC spanning die ontstaat door het fietsen gebruiken we ook om een UV-lamp aan te sturen die no-
dig is bij een andere puzzel.

## Elektrisch schema
Aangezien hier hier gebruik maken van een LCD die een voedingsspanning van 5V nodig heeft gebruiken we
een powerbank om onze esp te voeden. Deze 5V wordt namelijk rechtstreeks als voeding gebruikt voor ons LCD’tje en voedt m.b.v. een LDO, die we inbouwden in onze PCB, onze ESP32 die 3.3V nodig heeft

## PCB ontwerp
![PCB ontwerp fiets](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/blob/main/website/fiets_PCB.jpg)
## Code
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/blob/main/WERKENDE%20CODE%203-05/MeasuringDcVoltageWithCommunicationBuffer/src/main.cpp)

## realisatie