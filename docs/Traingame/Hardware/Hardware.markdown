---
layout: default
title: Hardware
parent: TrainGame
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

### Joystick
Deze joystick heeft vijf pinnen. Twee pinnen zijn voor de voeding: 5V en GND. De drie andere pinnen geven data door. De joystick kan over twee assen bewegen, de x-as en de y-as. Twee pinnen worden gebruikt om een signaal tussen 0 en 4095 te sturen, afhankelijk van de positie van de joystick. De laatste pin bevat het signaal van de drukknop, deze genereert een signaal dat 0 of niet 0 is, afhankelijk of de knop wordt ingedrukt of niet. We maken geen gebruik van de drukknop.

### LEDS

Voor de leds maken we gebruik van WS2811 LEDS. Deze hebben drie aansluitingen: 5 V, GND en data. Het datasignaal moet ook vijf volt bedragen. Het datasignaal van de ESP bedraagt 3,3 volt. We gebruiken een level shifter om het signaal te versterken. De LEDS kunnen elk apart worden aangestuurd. Ze zijn per vijftig verbonden


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
![ESP](/docs/TrainGame/Hardware/ESPsch.png)

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