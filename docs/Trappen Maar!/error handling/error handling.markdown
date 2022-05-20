---
layout: default
title: Error handling
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 7
---
# Error handling

## Mogelijke problemen omwille van hardware

Ondanks het correct ontwerpen, bestukken van de PCB’s en het correct implementeren van de code in de
ESP’s, kan het zijn dat er nog steeds enkele problemen optreden.
Zo kan het zijn dat de ingestelde fietssnelheid, nodig om niveau 4 te bereiken, te hoog/laag is bij een ander type hometrainer/dynamo.
Dit kan weliswaar makkelijk aangepast worden door in de code van de fiets, in de methode 'display()', de
voorwaarden voor een bepaald level aan te passen en te optimaliseren.
Een ander mogelijk probleem zou kunnen zijn dat het lastig is een bepaald fietsniveau aan te houden. Dit
kan verbeterd worden door met een grotere condensator te werken bij de brug-gelijkrichter.
Andere tips zijn:
* kabels LCD rechtstreeks solderen op de pinnen om slechte connecties te vermijden
* beginnen met een breadboard-implementatie om te verifiëren dat de grote componenten werken (dyname, ledstrips, LCD, ...)

## Mogelijke communicatieproblemen oplossen

Een groot probleem in de escape-room zou zijn dat de communicatie naar/van een bepaald onderdeel mislukt. Dit hebben we binnen "Trappen Maar" enigszins opgelost door rekening te houden met het falen van een (of meerdere) van de 7-segment displays (zie code Buffer). De fiets of buffer daarentegen zijn niet zo eenvoudig te vervangen aangezien deze een sleutelrol spelen en gelinkt zijn aan unieke hardware. Om dit te vermijden, hebben we die twee componenten intensief getest. Het is aangewezen dit eerste te doen via een esp development board om op die manier niet afhankelijk te zijn van andere componenten. 