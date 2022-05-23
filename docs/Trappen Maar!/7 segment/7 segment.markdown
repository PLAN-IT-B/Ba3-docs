---
layout: default
title: 7-segment displays
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 5
---
# 7-segment displays
## Algemeen
De 4 seven-segments displays in de escaperoom zullen in willekeurige volgorde een random getal tussen 1 en 4 laten zien met behulp van hun zeven ledstrips. 

## KiCad project
### Elektrisch schema
![](2022-05-13-21-30-33.png)
### PCB ontwerp
![](2022-05-13-21-30-54.png)
![](2022-05-13-21-31-06.png)
### Aansluitingen en onderdelen
Aan de 7-segment displays worden 7 ledstrips gehangen. We werken hiervoor met een kroonsteentje dat op de PCB bevestigd is zodat de ledstrips hier stevig aan vast hangen. De RGB ledstrips die wij gebruiken werken op 12V en hun RGB-lijnen functioneren als ground. We sturen deze aan met mosfets (van het type 2N7002) die we hoog/laag zetten en op die manier toe/open schakelen. 
### voedingsspanning
Voor het voeden van de 4 seven-segment displays maken we gebruik van drie 12V voedingen en een step-up/buck-boost convertor die in staat is de 5V output van een powerbank naar 12V te converteren([Klik hier voor convertor](https://www.tinytronics.nl/shop/nl/power/spanningsconverters/buck-boost-(step-up-down)-converters/usb-verstelbare-dc-dc-converter-3w-met-spanningsmeter)). Het voeden met een powerbank heeft als voordeel dat we de displays overal kunnen plaatsen en we dus geen rekening moeten houden met de aanwezigheid van stopcontacten.
## Code
Wij maakten in onze escapreroom gebruik van 4 seven-segment displays. Hierdoor moeten 3 spelers het getal dat oplicht zoeken in de kamer terwijl de laatste speler fietst (in het geval van 4 spelers). Dit zorgt ervoor dat het spel actiever wordt. Uiteraard is de keuze van aantal seven segmenten volledig vrij!   
In de link hieronder is de code terug te vinden voor 'segment 1'. Voor alle andere gebruikte displays is deze code identiek met uitzondering van het unieke client-ID dat in de code onder de volgende vorm teruggevonden kan worden:
```c
    client.connect("segment1")
```
In de code van de buffer moet bovendien ook onderstaande lijn code (twee keer) aangepast worden indien er niet met 4 seven-segmenten gewerkt wordt.
```c
int randomEsp = random(1,5); 
//dit zal segment 1,2, 3 of 4 roepen
```
De code van de displays is veel minder uitgebreid dan deze van de fiets en buffer. Hun voornaamste taak is namelijk het displayen van de nummers die de buffer doorstuurt. Deze ESP's luisteren naar volgende berichten: 

* '0', '1', '2', '3', '4': dit is het getal dat het display moet weergeven. Hier wordt de methode 'schakelLed' opgeroepen die de correcte ledstrips doet branden zodat het gewenste getal gevormd wordt. In deze methode worden ook de berichten 'led1', 'led2' en 'led3' gestuurd naar de fiets die hierdoor leds laat branden. Hiertussen zitten delays zodat het display een 5-tal seconden brandt. Hierna wordt de methode 'SendReady()' opgeroepen die naar de fiets het bericht 'send' stuurt en daarna de ledstrippen uitzet.
* resetSegment: dit wordt gestuurd door de buffer en reset het 7-segment display. 



[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/Segment1)

## realisatie
![](7segment.jpg)