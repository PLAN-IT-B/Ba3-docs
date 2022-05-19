---
layout: default
title: Communicatie
parent: Wristbands
grand_parent: Puzzels
nav_order: 3
---


# Communicatie

**Overzicht:**

- [Algemeen](#algemeen)
- [Bluetooth](#bluetooth)
- [MQTT](#mqtt)

## Algemeen

Dankzij het gebruik van de ESP, krijgen we de mogelijkheid om te communiceren met bluetooth en wifi. Hieronder vindt u een algemeen overzicht van de communicaties tussen de wristbands.

![Communicatie](SCHEMA_WRISTBANDS.JPG)

## Bluetooth

Voor de communicatie tussen onze Wristbands maken we gebuik van bluetooth. Dit doen we door gebruik te maken van het protocol en library genaamd BLECast (Bluetooth Low Energy). Via BLECast kan je pakketjes tot en met 765 bytes sturen. 

Voor onze toepassing sturen we een leeg bericht naar de andere wristbands. Vervolgens wordt door de wristband die het signaal ontvangen heeft de RSS van het signaal opgevraagd. RSS staat voor received signal strength, dit is vaak een negatief getal dat kleiner wordt hoe zwakker het signaal is.

## MQTT

![MQTT](MQTT.jpg)

Voor de communicatie tussen de andere puzzels maken we gebruik van een MQTT-broker, dit maakt gebruik van een publication en subscribe architectuur. Wij communiceren met Trappen Maar!, Eindpuzzel en Vuilbak. Een mooi overzicht tussen de communicatie met de andere modules vindt u hieronder.

![MQTT-chart](MQTT-chart.png)

### Publishen

Voor Trappen maar! publishen we naar ___"TrappenMaar/buffer"___. We kunnen 2 verschillende foutmelding doorgeven namelijk _"kleine fout"_ en _"grote fout"_ 

Voor Vuilbak publishen we naar ___"Wristbands/3cijfers"___. We sturen hier naar een viercijferige code, de naam is verwarrend aar dit komt omdat dit initieel een 3 cijferige code ging zijn dit is pas later 4 cijfers geworden.

<!--Voor het Eindspel publishen we naar ___"Eindspel/alarm"___. Dit zorgt er voor dat er telkens een alarm afspeelt als de partnerruil plaatsvindt.-->

### Subscriben

We luisteren ook naar verschillende directories omdat we graag van buitenaf de wristbands ook wat in de hand hebben. We luisteren naar **"controlpanel/reset"** en **"Wristbands"**.

Wanneer we *"Reset escaperoom"* of *"Reset Wristbands"* ontvangen, wordt er een resetfunctie opgeropen die onze wristband gaat resetten.

We kunnen de hartratesenor ook in een uitschakelen wanneer we *"hartslagsensor uit"* en *"hartslagsensor aan"* ontvangen. 

Stel dat er tijdens de escaperoom probelemen voordoen, kunnen we ook de wristbands volledig uitschakelen door via een MQTT *"Stop Wristbands"* te sturen. We kunnen die dan later eventueel terug inschakelen met de message "Herstart Wristbands".

