---
layout: default
title: Communicatie
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 1
---
# Communicatie

Zoals hierboven reeds gezegd begint elke ’ronde’ met het random genereren van een getal tussen 1 en 4
door de buffer en dit versturen naar een willekeurig seven segment. Dit seven segment toont dan dit getal
gedurende 5 seconden. Tijdens deze 5 seconden stuurt dit segment naar de fiets de boodschappen ”LED1”,
”LED2" en ”LED3”die de speler duidelijk maken hoeveel tijd hij of zij nog heeft om het correcte niveau te
fietsen. Wanneer de 5 seconden voorbij zijn roept het seven segment zijn methode ’sendready()’ op. In
deze methode wordt eerst ”send”gestuurd naar de fiets waarna de fiets het gefietste level doorstuurt naar de
buffer, hierna worden de ledstrips van dit segment uit gezet.   

Het is nu de buffer die het doorgekregen level van de fiets gaat controleren met het willekeurig getal tussen
1 en 4 dat hij zelf doorstuurde naar het seven segmetn. Indien dit correct is verhoogt hij zelf de score en
laat hij al dan niet een extra ledstrip branden die de score van de speler weergeeft. Ook stuurt de buffer
bij een correct gefietst niveau het bericht ”correct”naar de fiest waarna die een groene led laat branden of
een ”false”waarna een rode led gaat branden. Dit laatste uiteraard indien de speler te snel of te traag heeft
gefietst. De fiets stuurt als laatste, van deze cycli, het bericht ”newNumber”naar de buffer waarna alles
opnieuw begint en de buffer naar een random seven segment een willekeurig getal tussen 1 en 4 stuurt.