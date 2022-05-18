---
layout: default
title: Het Spel
parent: De vuilbak
grand_parent: Puzzels
nav_order: 1
---

# Het spel

## Starten van de escaperoom

Aan het begin van de escaperoom is de vuilbak niet actief. Deze wordt pas actief wanneer de energiebuffer in de groene zone staat.
In deze staat staat de LCD uit, en ook het indrukken van de knoppen op het toetsenbord of de knoppen op de scanplatformen doet niets.
Wanneer de puzzels "Wristbands" en "Traingame" worden opgelost, wordt er een code vrijgegeven die naar de vuilbak wordt gestuurd. De eerste vier cijfers van de code worden door "wristbands" verkregen, het laatste cijfer van de code wordt door "Traingame" verkregen. Vooraleer deze opdrachten opgelost worden kan men geen juiste code ingeven doordat de ingestelde code dan [-1,-1,-1,-1,-1] is.

## Genoeg energie
Wanneer de energie-buffer in het groene gebied komt zal de vuilbak actief worden. Het scherm gaat aan en het eerste dat de deelnemers moeten doen is de juiste code ingeven. Foute pogingen worden afgestraft door energieverlies. Wanneer de juiste code is ingegeven kan de puzzel beginnen.

## Puzzel
Doorheen de escape room ligt er vuilnis verstopt met daarop RFID-tags. Deze dienen op het juiste scanplatform gelegd te worden(PMD, rest of papier & karton) en vervolgens moet er op de bijhorende scanknop gedrukt worden.
Indien het afval juist gescand werd klinkt er een succesgeluidje en moet het afval in de juiste vuilbak gegooid worden, dit zal worden gedetecteerd door de gewichtssensoren.
Wanneer er een fout gemaakt wordt, wordt dit aan de buffer gemeld en gaat er een beetje energie van de buffer. Om dit te laten weten aan de spelers zal er het foutgeluidje afspelen. 

## Einde
Wanneer al het afval juist gesorteerd werd, wordt het totale gewicht per vuilniszak afgebeeld op de LCD. De code zal de som zijn van deze gewichten. Deze code zal ook verzonden worden naar de eindpuzzel.
Dit beeld zal op het scherm blijven staan zolang de buffer in het groene gebied blijft.
