---
layout: default
title: Errorhandling
parent: Wristbands
grand_parent: Puzzels
nav_order: 6
---

# Error handling

**overzicht:**

- [Communicatie wristbands onderling](#communicatie-wristbands-onderling)
- [Velcrostrips](#velcrostrips)
- [De hartslagsensor](#de-hartslagsensor)
- [De batterijswitch](#de-batterijswitch)


## Communicatie wristbands onderling

Een van de grote problemen is dat er geen MQTT communicatie tussen de wristbands onderling gebeurd. Dit heeft als gevolg dat wanneer er een probleem voordoet met 1 wristband, de volledige puzzel dan moet worden uitgeschakeld. Gelukkig kunnen we dit wel doen door middel van de externe controle.

## Velcrostrips

Doordat de modules op de arm worden gedragen, worden ze ook veel bewogen. Dit zorgt er voor dat de velcro stevig moet worden gemonteerd op de doosjes. Eerst waren deze met enkel een lijmpistool gelijmd, maar we hebben deze later verstevigd met wat ducktape. Hierdoor waren ze zeer stevig. Wanneer de spelers beginnen te zweten laat dit wel wat sporen na op de ducktape.

## De hartslagsensor

Aangezien we de hartslagsensor smd hebben gesoldeerd, was het een uitdaging om deze sensor dicht bij de arm te krijgen. Het zou misschien beter zijn om de hartslagsensor iets verder van de pcb te kunnen brengen door eventueel een extra pcb tussen de hartslagsensor en de grote pcb te steken. Dit zou de hartslagsensor iets dikker maken, waardoor we deze dichter bij de arm zouden kunnen krijgen.

Een tweede puntje is dat de hartslagsensor de arm af en toe niet herkent, terwijl de spelers de wristband wel aan hebben. Dit is natuurlijk het gevolg van het feit dat de sensor te ver is van de arm. We hebben dit opgelost door de mogelijkheid te voorzien om de hartslagsensor uit te schakelen via de externe controle.

## De batterijswitch

Omdat we ons tijdens het ontwerp van de pcb niet echt hebben gefocused op de batterij zijn we deze wat uit het oog verloren. Hierdoor hadden we pas later ontdekt dat het goed zou zijn om een switch te voorzien tussen de batterij en ons pcb, zodat we deze kunnen uitschakelen. De montage van deze switch is iets minder goed gedaan. De mogelijkheid bestaat namelijk dat een kabeltje afbreekt. Het zou dus beter zijn als er nog een switch wordt voorzien op het pcb zelf. Dit zou dit alles iets steviger maken.

