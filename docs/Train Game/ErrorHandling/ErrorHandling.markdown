---
layout: default
title: ErrorHandling
parent: Train Game
grand_parent: Puzzels
nav_order: 6
---

# Errorhandling
Onze puzzel bestaat uit één groot geheel. Alles is netjes geïmplementeerd in het bord. Elk element is onvervangbaar: zonder joystick kan men de trein niet besturen, zonder LEDs kan men geen trein weergeven en zonder LCD zullen de spelers niet weten wat ze moeten doen. Dit zorgt ervoor dat error handling in de code zelf niet erg nuttig is. Alles is stevig gemonteerd en de kans dat er iets misgaat is klein. Toch willen we vermijden dat een fout in onze puzzel het spel te grote gevolgen heeft voor het volledige spel. Stel dat onze puzzel in het worst case scenario niet werkt dan kunnen we via een externe module signalen sturen naar de andere puzzels die van ons signalen verwachten.  