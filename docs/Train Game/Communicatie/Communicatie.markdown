---
layout: default
title: Communicatie
parent: Train Game
grand_parent: Puzzels
nav_order: 5
---
# Communicatie

Communicatie is essentiëel voor het verloop van het spel. Sommige puzzels kunnen pas gespeeld worden als er andere zijn opgelost. We communiceren met drie puzzels aan de hand van de MQTT server. Onze puzzel moet zowel statusupdates kunnen versturen als verzenden. We maken gebruik van de "client.publish" en "client.subscribe" commandos. 


|:----:|
|![Communicatie schema](Communicatie_cut.png)|
|**Commmunicatie schema**|

## Eindpuzzel
We ontvangen het resetsignaal van de eindpuzzel. Hiervoor luisteren we naar de “controlpanel/reset” directory. Bij het ontvangen van een resetsignaal komt onze puzzel terug in zijn begintoestand terecht. De PCB wordt softwarematig gereset, dit is te vergelijken met een druk op de resetknop. Eenmaal gereset stuurt onze puzzel een antwoord zodat de eindpuzzel node weet dat er gereset werd en dat onze game klaarstaat. We sturen "Traingame Ready" naar de “controlpanel/status” directory. 


## Buffer
Afhankelijk van het bufferniveau kunnen de spelers wel of niet spelen. We luisteren hiervoor naar de "TrappenMaar/zone" directory. De buffer geeft mee welk niveau hij heeft. Als we de boodschap "groen" of "oranje" ontvangen kan er gespeeld worden. Maar als we "rood" ontvangen blokkeert onze puzzel en moet er eerst opnieuw gefietst worden. Als de spelers fouten maken dan worden ze hiervoor gestraft.
We sturen "TrappenMaar/buffer" naar "kleine fout" of "grote fout" om de buffer te laten zakken.




## Garbage
Als de spelers de eerste puzzel oplossen krijgen ze hiervoor het laatste cijfer van de code om de vuilbak te openen. We publishen "CodeChar" naar "treingame/4decijfer". "CodeChar" bevat een stuk tekst met het juiste cijfer. Om de tweede fase te ontgrendelen moet eerst de code worden ingegeven bij de vuilnisbak. We luisteren naar de "garbage/status" directory. Vanaf dat we hier een update ontvangen kan het tweede deel gespeeld worden.
