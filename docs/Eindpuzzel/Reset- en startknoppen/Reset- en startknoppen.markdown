---
layout: default
title: Reset- en startknoppen
parent: Eindpuzzel
grand_parent: Puzzels
nav_order: 2
---
 
# De reset- en startknoppenn

---

## Waarom

---

Als we escaperoom willen starten, moeten we eerst alles resetten. Dit zou veel werk zijn om handmatig te doen bij elke esp. Om deze tijd uit te sparen kwamen we tot de functionaliteit van de reset knop. 

De startknop is vanzelfsprekend handig. Voor het startsignaal te geven en voor de timer starten.
Er worden namelijk twee startsignalen gestuurd. Een startsignaal naar de wristbands voor hun leds aan te zetten. Maar ook een startsignaal naar de garbage puzzel omdat zij via hun speaker een buzz lawaai gaan laten afspelen.

---
## Wat doen ze
---

De reset knop stuurt een message *Reset escaperoom* naar een bepaalde directory. Iedereen luistert naar deze directory, en iedereen zal resetten. Nadat elke individuele puzzel klaar is, zal elke puzzel dit signaleren. Als elke esp in de escaproom dit correct uitvoert, weten we dat de escaproom klaar is om te spelen. De led van de startknop zal licht geven, en de startknop zal werken.

De startknop is eenvoudiger. Bij het indrukken gaan we eerst kijken of de escaperoom ready is. Zoja, zal de solenoïde van de puzzlebox 30 seconden onder spanning staan, en de algemene timer zal starten. De reden dat de solenoïde 30 seconden intrekt is zodat de spelers de tijn hebben om hun gsms in een voorziene ruimte in de puzzlebox te steken. Deze luik zal pas terug open gaan als de escaperoom volledig is opgelost.

**Flowcharts**

![](Blokschema-Reset-Start.svg)
[Klik hier voor deze figuur in het groot.](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/main/Documentatie%20eindpuzzel/Blokschema-Reset-Start.svg)