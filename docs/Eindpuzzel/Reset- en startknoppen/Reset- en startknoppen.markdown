---
layout: default
title: Reset- en startknoppen
parent: Eindpuzzel
grand_parent: Puzzels
nav_order: 2
---
 
# De reset- en startknoppenn

---

## Waarom?

---

We kwamen tot de conclussie dat het veel werk zou zijn, om steeds handmatig alle esps te resetten, voor het spelen van de escape room. Om deze tijd uit te sparen kwamen we tot de functionaliteit van de reset knop. 

De startknop is vanzelfsprekend handig. Voor het startsignaal te geven en voor de timer starten.
(Er word ook een startsignaal gestuurd naar de wristbands omdat dan de leds van hun bandjes zullen aangaan.)

---
## Wat doen ze?
---

De reset knop stuurt via de mqtt broker een message *Reset escaperoom* naar een bepaalde directory. Iedereen luistert naar deze directory, en iedereen zal resetten. Nadat elke individuele puzzel klaar is, zal elke puzzel dit signaleren. Als elke esp in de escaproom dit correct uitvoert, weten we dat de escaproom klaar is om te spelen. De led van de startknop zal licht geven, en de knop zelf zal werken.

De startknop is eenvoudiger. Bij het indrukken gaan we eerst kijken of de escaperoom ready is. Zoja, zal de solenoïde relais van de puzzlebox 30 seconden schakelen, en de algemene timer zal starten. De reden dat de solenoïde 30 seconden intrekt is zodat ze hun gsms in een voorziene ruimte in de puzzlebox kunnen steken. Deze luik gaat pas terug open als de escaperoom volledig is opgelost.

**Flowcharts**

![](Flowchart_reset-start_werking.png)