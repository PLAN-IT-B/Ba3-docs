---
layout: default
title: 7 segment
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 4
---
# 7 segment
## Algemeen
nog uitleg

## KiCad project
Voor het voeden van de 4 seven segments maken we gebruik van twee 12V pc voedingen en twee step-up/buck-boost convertoren (die 5V naar 12V converteren).[Klik hier om het te bestellen.](https://www.tinytronics.nl/shop/nl/power/spanningsconverters/buck-boost-(step-up-down)-converters/usb-verstelbare-dc-dc-converter-3w-met-spanningsmeter). Aan deze convertoren hingen we dan een powerbank wat als voordeel had dat we de PCB ergens konden plaatsen en geen rekening moesten houden met een stopcontact.
### Elektrisch schema
![](2022-05-13-21-30-33.png)
### PCB ontwerp
![](2022-05-13-21-30-54.png)
![](2022-05-13-21-31-06.png)
## Code
Wij maakten in onze escapreroom gebruik van 4 seven segments. Hierdoor moet er een speler het getal dat oplicht zoeken in de kamer en een andere speler fietsen. Dit zorgt ervoor dat het spel actiever wordt. Uiteraard is de keuze van aantal seven segmenten volledig vrij! Hieronder is een code teruggevonden van ons 'segment 1', voor alle andere segmenten is deze code identiek met uitzondering van het unieke client idee dat in de code onder de volgende vorm teruggevonden kan worden.
```c
    client.connect("segment1")
```
In de code van de buffer moet bovendien ook volgende lijn code aangepast worden:
```c
int randomEsp = random(1,5); 
//dit zal segment 1,2, 3 of 4 roepen
```
Bovenstaande lijn komt twee keer in de code buffer en moet gebruikt worden indien er 4 seven segmenten in het spel zitten. 

[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/Segment1)

## realisatie
FOTO TOEVOEGEN