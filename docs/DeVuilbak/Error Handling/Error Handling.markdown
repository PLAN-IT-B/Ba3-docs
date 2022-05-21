---
layout: default
title: Error Handling
parent: De vuilbak
grand_parent: Puzzels
nav_order: 10
---
# Error Handling
## Mogelijke problemen omwille van hardware
### RFID scanner
Tijdens het testen hebben we gemerkt dat soms enkel de restafval scanner het niet doet. Na een reset werkt dit wel weer. We vermoeden dat dit komt doordat onze draadjes niet robuust genoeg verbonden zijn en dit soms even los komt waardoor de communicatie wordt verbroken, hierdoor zal de scanner niet meer weten wanneer hij moet scannen. Normaal gezien zou dit geen probleem mogen zijn aangezien de scanner via het resetsignaal na elke scan weer wordt gereset maar omdat wij deze niet hebben verbonden kan hij dit niet doen. Een mogelijkheid om dit te verbeteren zou zijn om draadjes rechtstreeks aan de RFID module te solderen of om de reset wel te verbinden. Helaas kunnen we deze fout niet opvangen.

### Vuilnis 
Het is mogelijk om nadat er vuilnis in een zak is gegooid dit er terug uit te halen of nadat het vuilnis is gescand een ander stuk vuil in de zak gooien. Het eerste probleem zouden we kunnen oplossen door te detecteren of er gewichtsverlies is, echter is dit moeilijk te implementeren aangezien de waarden soms fluctueren tussen enkele grammen en sommige stukken vuilnis redelijk licht zijn. We zouden er voor kunnen zorgen dat wanneer er niets gescand is de vuilnisbakken fysiek toe gaan.

Het probleem dat er na het scannen en ander stuk vuil in de zak kan gesmeten worden kan worden opgelost door aan elke tag een bepaald gewicht te koppelen dat overeen komt met het voorwerp waar de tag aan zit.

Uiteindelijk zullen deze methodes om het spel te omzeilen niet direct voor problemen zorgen in het verdere verloop van de escaperoom aangezien op het einde de totale gewichten slechts 1x worden gelezen en deze waarden worden afgebeeld op het LCD.

