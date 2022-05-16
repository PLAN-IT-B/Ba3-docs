---
layout: default
title: Communicatie
parent: Wristbands
grand_parent: Puzzels
nav_order: 3
---


# Communicatie
![Communicatie](SCHEMA_WRISTBANDS.JPG)
## Bluetooth
Voor de communicatie tussen onze Wristbands maken we gebuik van bluetooth. Dit doen we door gebruik te maken van het protocol en library genaamd BLECast. Via BLECast kan je pakketjes tot en met 765 bytes sturen. 

Voor onze toepassing sturen we een leeg bericht naar de andere wristbands. Vervolgens wordt door de wristband die het signaal ontvangen heeft de RSS van het signaal opgevraagd. RSS staat voor received signal strength, dit is vaak een negatief getal dat kleiner wordt hoe zwakker het signaal is.

## MQTT
![MQTT](MQTT.jpg)
Voor de communicatie tussen de andere puzzels maken we gebruik van een MQTT-broker, dit maakt gebruik van een publication en subscribe architectuur. Wij communiceren met Trappen Maar!, Eindpuzzel en Vuilbak.

Voor Trappen maar! publishen we naar ___"TrappenMaar/buffer"___. We kunnen 2 verschillende foutmelding doorgeven namelijk _"kleine fout"_ en _"grote fout"_ 

Voor Vuilbak publishen we naar ___"Wristbands/3cijfers"___. We sturen hier naar een viercijferige code, de naam is verwarrend aar dit komt omdat dit initieel een 3 cijferige code ging zijn dit is pas later 4 cijfers geworden.

Voor het Eindspel publishen we naar ___"Eindspel/alarm"___. Dit zorgt er voor dat er telkens een alarm afspeelt als de partnerruil plaatsvindt.