---
layout: default
title: Hardware
parent: Wristbands
grand_parent: Puzzels
nav_order: 3
---
# Hardware

>[Visit our Github to find our Kicad6.0 project!](https://github.com/PLAN-IT-B/BachelorProefWristbands/tree/main/KiCad-project)

**Overzicht:**

- [Algemeen](#algemeen)
- [Componenten](#componenten)
- [Elektrisch Schema](#elektrisch-schema)
- [PCB-design](#pcb-design)

## Algemeen
Aangezien we gekozen hebben voor de wristbands, was het belangrijk dat we ons design zo compact mogelijk hielden. Daarvoor wilden we er voor zorgen dat de wristbands konden communiceren met de speler zonder dat we grote LCD-schermen moesten gebruiken.

## Componenten


| Component   |      Type      |  Gebruik |
|----------|:-------------:|------:|
| mircroprocessor|  ESP32_WROOM_32UE| Dit is de microprocessor die als het ware het brein vormt van ons ontwerp. Deze esp heeft de mogelijkheid om een externe antenna toe te voegen. |
| Antenna |A24-HABUF-P5I-ND   |   We maken gebruik van de antenna zodat we accurater de RSS-waarde van het signaal kunnen bepalen. |
| RGB-LED | SMD 5050 |    We maken gebruik van deze Led om te laten zien aan de speler wanneer die een fout in afstand, maar ook om een code te sturen of te laten weten dat ze hun wristband niet goed aanhebben. |
| Infrarood/Hartslagsensor | MAX30102 |  Met deze sensor kunnen we bekijken of de spelers hun wristband effectief aanhebben. |
| Oplaadbare batterij | LiPo 802035 |    Dit gebruiken we als voeding voor onze pcb. Deze batterij heeft een voltage van 3.7V en een capaciteit van 500mAh. |
| LDO | ncp1117  |    Deze LDO zorgt ervoor dat we onze esp en hartslagsensor kunnen voeden met 3,3V. |
| Weerstanden en condensatoren | SMD0805 | Dienen als pull-up weerstanden en ontkoppelcondensatoren, maar ook voor het aansturen van de LED|
| Buttons | standard buttons |    We gebruiken standaard switches in ons ontwerp. Bij ons zijn deze trough-hole, omdat die steviger zijn op het pcb bord. De switches dienen ervoor te resetten en te programmeren. |
|Switches| MS180 |We gebruiken een switch om de batterij te verbinden met ons PCB. Hierdoor moeten kunnen we alles solderen een moeten we niks losmaken.|

### LED

De LED wordt aangestuurd via 3 GPIO-pinnen, namelijk pin2 voor groen, pin4 voor blauw en pin16 voor rood. Voor dat deze lijnen de LED bereiken worden ze nog onderbroken door een weerstand van 68 Ohm

### HARTSLAGSENSOR

De hartslagsensor is slechts verbonden via 4 van de 8 pinnen aan ons pcb. We hebben hier de enerzeids de voeding en de ground, en anderzijds een SCL (serial clock) en een SDA (serial data). Deze laatste 2 zijn nodig voor de communicatie met de ESP via een I2C-bus.


## Elektrisch Schema
Voor het pcb ontwerp hebben we gebruik gemaakt van KiCad. Hierin hebben we een elektrisch schema ontworpen die we vooraf hebben uitgeprobeerd op een breadbord. De uitkomst vind je in onderstaand schema.
![](elektrisch-schema.png)
Je ziet dat we 4 condensatoren gebruiken om onze bron te ontkoppelen. In de praktijk gebruikten we maar 2 van deze, namelijk diegene die het dichts bij de ESP bevinden. 

[Elektrisch schema in het groot](https://github.com/PLAN-IT-B/BachelorProefWristbands/blob/main/KiCad-project/Afbeeldingen%20PCB/Elektrisch-schema.png?raw=true)


## PCB-design
In volgende twee figuren ziet u een afbeelding van ons PCB-design. We hebben ervoor gekozen om dubbelzijdig te werken om opnieuw een compact design te realiseren. 

|front | back |
|:----: |:----: |
|![](pcb-front.png)|![](pcb-back.png)|
| [PCB-front in het groot](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefWristbands/main/KiCad-project/Afbeeldingen%20PCB/pcb-front.png)| [PCB-back in het groot](https://raw.githubusercontent.com/PLAN-IT-B/BachelorProefWristbands/main/KiCad-project/Afbeeldingen%20PCB/pcb-back.png)|