---
layout: default
title: Communicatie
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 2
---
# Communicatie

Elke ronde van het fietsspel begint met het random genereren van een getal tussen 1 en 4
door de buffer en die stuurt dit naar een willekeurig seven-segment display. Het display toont vervolgens het getal
gedurende 5 seconden. Tijdens deze 5 seconden stuurt dit segment naar de fiets de boodschappen ”LED1”,
”LED2" en ”LED3” die de speler duidelijk maken hoeveel tijd hij of zij nog heeft om het correcte niveau te
fietsen. Wanneer de 5 seconden voorbij zijn, roept het seven-segment display zijn methode ’sendready()’ op. In
deze methode wordt eerst ”send” gestuurd naar de fiets waarna de fiets het gefietste level doorstuurt naar de
buffer, hierna worden de ledstrips van dit segment uit gezet.   

Het is nu de buffer die het doorgekregen level van de fiets gaat controleren met het willekeurige getal dat hij zelf doorstuurde naar het display. Indien dit correct is, verhoogt hij zelf de score en
laat hij een extra ledstrip branden die de score van de speler weergeeft. Ook stuurt de buffer
bij een correct gefietst niveau het bericht ”correct” naar de fiets waarna die een groene led laat branden of ”false” waarna een rode led gaat branden. Dit laatste uiteraard indien de speler niet aan het correcte snelheidsniveau fietste. De fiets stuurt tenslotte het bericht ”newNumber” naar de buffer waarna de volgende ronde van start kan gaan.

Het laatste deel van de communicatie, is die naar andere puzzels toe. Van zodra de buffer tot een nieuw kleurniveau werd opgeladen (of ontladen), wordt dit doorgegeven aan de andere puzzels. Omgekeerd kunnen ook andere puzzels aan de buffer laten weten dat er een fout werd gemaakt. Afhankelijk van de ernst van die fout, klein of groot, zal de bufferwaarde met 1, respectievelijk 2 waarden verminderd worden.

## Flowchart
![](flowchart.png)