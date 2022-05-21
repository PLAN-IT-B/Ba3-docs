---
layout: default
title: Software
parent: Eindpuzzel
grand_parent: Puzzels
nav_order: 3
---


# Software

Er is veel werk gestoken in de commentaren van de codes. Voor uitleg van de verschillende delen en functies kunt u op het gemak door de codes gaan op github.

De programmas zijn allebij gebasseerd op de bassiscode van een cijferslot met 3x4 keypad en lcd display. Maar ook gebasseerd op de bassiscode voor verbinding met wifi en mqtt broker gekregen van de onderzoeksgroep dramco tijdens de projectweek. Ondanks dat de programmas dit gemeenschappelijk hebben. Zijn ze voor de rest volledig verschillend.

Er valt op te merken in de codes dat we vaak booleans gebruik die triggeren in de loop. Dit doen we omdat we de Interupt Service Handlers altijd zo kort mogelijk willen houden. Dit is een goeie gewoonte zodat er voldoende tijd is voor andere interrupts (zoals de timer interrupt) te verwerken.

|[Code van UV-slot ](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/blob/main/Documentatie%20UV-slot/UV_Slot%20-%20code/src/main.cpp)| [Code van de eindpuzzel](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/blob/main/Documentatie%20eindpuzzel/Eindslot%20-%20code/src/main.cpp)|


De flow van het UV-slot is de werking van de UV-slot code.  
De flow van het eindslot is de werking van de einpuzzel code.

![](Flow_eindpuzzel.svg)

Maar de code van de eindpuzzel voert ook deze werking correct uit!

![](Flowchart_reset-start_werking.png)

---
## Error handling
---

Er is een externe computer verbonden met een esp devkit, die alle communicatie berichten kan sturen moest het niet gebeuren door een bepaalde puzzel. Deze esp devkit luisterd ook naar alle directories van de mqtt broker.
**Zo kunnen we alle communicatiefouten detecteren en corrigeren. Maar ook een puzzel simuleren voor als er hardware/software fouten zijn.**

Na de testfase door verschillende groepen bleek dit steeds minder nodig te zijn, tot op een punt dat de volledige escaperoom perfect werkte zonder hulp van deze esp.