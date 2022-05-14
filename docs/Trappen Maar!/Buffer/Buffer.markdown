---
layout: default
title: Buffer
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 3
---
# Buffer
## Algemeen
De buffer bestaat uit 16 ledstrips waarvan de onderste 5 rood zijn, de middelste 6 oranje en de bovenste 5
groen. Afhankelijk van de score van de spelers gaat er een ander aantal ledstrips branden en komt het spel in
een bepaalde kleurenzone (rood, oranje of groen) terecht. De kleurenzone bepaalt welke andere escaperoom spelletjes
gespeeld kunnen worden/ welke spelletjes geblokkeerd zijn.

## KiCad project
Aangezien we hier met ledstrips werken zijn we genoodzaakt gebruik te maken van 12V. We werken hier
dus met een 12V pc voeding die rechtstreeks onze ledstrips voedt en die via een buck-converter en LDO
(ingebouwd in onze PCB) onze ESP voedt met 3V3.

### Elektrisch schema
![](2022-05-13-21-31-35.png)
### PCB ontwerp
![](2022-05-13-21-31-51.png)
![](2022-05-13-21-32-12.png)
## Code
In onze code worden bepaalde fouten, zoals een verloren bericht en connectie, opgevangen. Wanneer de
buffer namelijk na 15 seconden nog geen nieuw bericht ”newNumber” gekregen heeft zou het kunnen dat een
seven segment niet meer werkt/het bericht verloren is gegaan en indien dit het geval is, produceert de buffer
zelf een nieuw random getal naar een willekeurig seven segment. 
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/sender)
## realisatie
FOTO TOEVOEGEN