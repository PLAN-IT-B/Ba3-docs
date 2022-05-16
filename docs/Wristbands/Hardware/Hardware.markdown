---
layout: default
title: Hardware
parent: Wristbands
grand_parent: Puzzels
nav_order: 3
---
# Hardware
[Visit our Github to find our Kicad project!](https://github.com/PLAN-IT-B/BachelorProefWristbands/tree/main/KiCad-project)
## Algemeen
Aangezien we gekozen hebben voor de wristbands, was het belangrijk dat we ons design zo compact mogelijk hielden. Daarvoor wilden we er voor zorgen dat de wristbands konden comuniceren met de speler zonder dat we grote LCD-schermen moesten gebruiken.

## Componenten

**ESP32_WROOM_32UE** Dit is de microprocessor die als het ware het brein vormt van ons ontwerp. Deze esp heeft

**Antenna:** We maken gebruik van de antenna zodat we accurater de RSS van het signaal kunnen bepalen.

**RGB-LED:** We maken gebruik van deze Led om te laten zien aan de speler wanneer die een fout in afstand, maar ook om een code te sturen of te laten weten dat ze hun wristband niet goed aanhebben.

**Infrarood/Hartslagsensor:** Met deze sensor kunnen we bekijken of de spelers hun wristband effectief aanhebben.

**Lithium Polymer Rechargeable Battery:** Dit gebruiken we als voeding voor onze pcb. Deze batterij geeft een voltage van 3.7V en een capaciteit van 1800mAh.

**LDO ...:** Deze LDO zorgt ervoor dat we onze esp en hartslagsensor kunnen voeden met 3.3V.

**Weerstanden en condensatoren:** We maakten gebruik van smd-componenten van de serie 0805. De weerstanden komen bovendien uit de E12 reeks.

**Switches** We gebruiken standaard switches in ons ontwerp. Bij ons zijn deze trough-hole, omdat die steviger zijn op het pcb bord. De switches dienen ervoor te resetten en te programmeren.

## Elektrisch Schema
Voor het pcb ontwerp hebben we gebruik gemaakt van KiCad. Hierin hebben we een elektrisch schema ontworpen die we vooraf hebben uitgeprobeerd op een breadbord. De uitkomst vind je in onderstaand schema.
![](elektrisch-schema.png)
Je ziet dat we 4 condensatoren gebruiken om onze bron te ontkoppelen. In de praktijk gebruikten we maar 2 van deze, namelijk diegene die het dichts bij de ESP bevinden. 


## PCB-design
In volgende twee figuren ziet u een afbeelding van ons PCB-design. We hebben ervoor gekozen om dubbelzijdig te werken om opnieuw een compact design te realiseren. 
### front
![](pcb-front.png)
### back
![](pcb-back.png)
