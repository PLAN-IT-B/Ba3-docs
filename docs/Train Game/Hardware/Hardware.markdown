---
layout: default
title: Hardware
parent: Train Game
grand_parent: Puzzels
nav_order: 3
---
# Hardware
## Componenten
We bouwden een grote kaart waarop de treintrajecten en steden werden weergegeven aan de hand van LEDs. De spelers kunnen de trein besturen aan de hand van de joystick. Voor elke game is er een apart logo dat wordt afgebeeld op een LCD, als kleine hint. Hieronder vindt men een lijst met de gebruikte componenten. Het PCB ontwerp wordt verderop besproken.

- Joystick              : KY-023 Joystick module (XY-Axis)
- Ledkaart              : WS2811 Digitale 8mm RGB LED String
- Lcd scherm            : WELK MODEL???
- PCB                   : 
- Voeding               : CS1202000 AC-DC convertor 12V 2A
- Gelasercut spelbord   : Kartonplaat


## PCB

We voeden ons ontwerp met een adapter die 12 V en 2 A voorziet. We vormen deze spanning om tot een spanning van 5 V aan de hand van een buck convertor. Deze spanning wordt op zijn beurt omgevormd door een spanningsregelaar. Alle externe componenten worden aangesloten aan de hand van pinheaders. Deze zijn rechtstreeks of onrechtstreeks verbonden met de ESP32-WROOM. Om de leds aan te sturen maken we gebruik van een data signaal. De ESP stuurt signalen uit van 3.3V maar de LEDS hebben een datasignaal van 5 V nodig. We gebruiken een level shifter om het signaal te versterken. Verder maken we nog gebruik van weerstanden en condensatoren.
Hieronder volgt een lijst met componenten.

- Spanningsregelaar: AMS1117
- Level shifter: BSS138
- Boot/Enable switches: ?????
- Barrel jack: ???
- Buck convertor
- Pin header    
- Condensatoren
- Weerstanden



### ESP 
![ESP](ESPsch.png)

### Buck convertor & voeding
![BUCK CONVERTOR](Buck12V.png)

### Aansluiting pinheaders
![PINOUT](Pinheaders.png)

### Spanningsregelaar
![SPANNINGSREGELAAR](spanningsregelaar.png)


### PCB

Het ontwerp van de PCB bevindt zich hieronder. Er zijn nog een paar aanpassingen gebeurt om fouten op te lossen, deze zullen we verder verklaren. 
![](PCBfront.png)
![](PCBback.png)