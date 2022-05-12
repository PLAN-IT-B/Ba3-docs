---
layout: default
title: 7 segment
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 4
---
# 7 segment
## Algemeen



## Elektrisch schema
Voor het voeden van de 4 seven segments maken we gebruik van 4 12V pc voedingen, op dezelfde manier
zoals bij de buffer

## PCB ontwerp

## Code
In onze code worden bepaalde fouten, zoals een verloren bericht en connectie, opgevangen. Wanneer de
buffer namelijk na 15 seconden nog geen nieuw bericht ”newNumber”gekregen heeft zou het kunnen dat een
seven segment niet meer werkt/het bericht verloren is gegaan en indien dit het geval is, produceert de buffer
zelf een nieuw random getal naar een willekeurig seven segment.

## realisatie