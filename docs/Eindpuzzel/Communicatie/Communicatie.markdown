---
layout: default
title: Communicatie
parent: Eindpuzzel
grand_parent: Puzzels
nav_order: 5
---


# Communicatie

![](Netwerktopologie_escaperoom.svg)

[Klik hier voor deze figuur in het groot.](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/main/Documentatie%20communicatie/Netwerktopologie%20escaperoom.svg)


## UV-slot:

---

De puzzel **luistert** naar twee directories:
* *"garbage/eindcode"* -> De garbage puzzel zal 4 cijfers doorsturen die de code zal zijn van het cijferslot.
* *"controlpanel/reset"* -> Als er hier een bericht "Reset escaperoom" zal worden gepost. Zal deze esp zich resetten.

De puzzel zal voor zijn werking naar één directory **publishen**:
* *"controlpanel/status"* -> Na het correct en volledig opstarten van de uv-slot puzzel. Zal hier het bericht, “UV-slot Ready”,  gestuurd worden.



## Eindpuzzel:

---

Deze esp zal **luisteren** naar één directory:
* *"controlpanel/status"* -> Hier zullen we teweten kommen welke esp's allemaal ready zijn.

Deze esp zal voor zijn werking naar twee directories **publishen**:
* *"controlpanel/reset"* -> Wanneer de blauwe startup knop wordt ingedrukt. Zal het hier het bericht "Reset escaperoom" sturen.
* *"Wristbands"* -> Nadat de gsms in het bakje zitten, zal de solenoïde sluiten, en zal er ook een startsignaal gestuurd worden, zodat op dat moment pas de leds van de wristbands aan gaan.
* *"garbage"* -> Nadat de gsms in het bakje zitten, zal de solenoïde sluiten, en zal er ook een startsignaal gestuurd worden, zodat op dat moment de speaker van de garbage puzzel een buzz lawaai kan maken om aan te duiden dat de game begonnen is. 
* *"TrappenMaar/buffer"* -> Wanneer er voldoende energie is, en de code wordt fout ingegeven. Dan zullen er hier 6 grote fouten worden naar gestuurd.
