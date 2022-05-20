---
layout: default
title: Software
parent: Eindpuzzel
grand_parent: Puzzels
nav_order: 3
---


# Software

Er is veel werk gestoken in de commentaren van de codes. Voor uitleg van delen en functies kunt u op het gemak door de codes gaan op github.

De programmas zijn allebij gebasseerd op de bassiscode van een cijferslot met 3x4 keypad. Maar ook gebasseerd op de bassiscode voor verbinding met wifi en mqtt broker gekregen van de onderzoeksgroep dramco tijdens de projectweek. Ondanks dat de programmas dit gemeenschappelijk hebben. Zijn ze voor de rest volledig verschillend.

Er valt op te merken in de codes dat we vaak booleans gebruik die triggeren in de loop. Dit doe ik vaak omdat we de Interupt Service Handlers altijd zo kort mogelijk willen maken. Dit is een goeie gewoonte zodat er voldoende tijd is voor andere interrupts (zoals de timer interrupt) te verwerken.

| [Code van UV-slot ](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/blob/main/Documentatie%20UV-slot/UV_Slot%20-%20code/src/main.cpp)| [Code van de eindpuzzel](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/blob/main/Documentatie%20eindpuzzel/Eindslot%20-%20code/src/main.cpp)|