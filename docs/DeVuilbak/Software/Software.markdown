---
layout: default
title: Software
parent: De vuilbak
grand_parent: Puzzels
nav_order: 3
---
# Software
## Verschillende staten
Om de code overzichtelijk te maken hebben we gewerkt met verschillende staten. In welke staat de vuilbak zich bevind zal telkens in de loop bekeken worden aan de hand van booleans die veranderen. Deze booleans zijn:
* reset: Moet de ESP gereset worden?
* energie: Is er genoeg energie? Staat de buffer in de groene zone?
* actief: Is de code al eens juist ingegeven?
* checkVuilnisTotaal: Is al het vuilnis al gevonden?
* wachtOpGewicht: Is er al gewicht gedetecteerd?

Werken met deze staten was zeer handig aangezien er hierdoor geen probleem ontstond wanneer het niveau plots niet meer groen was. Het programma zou gewoon naar zijn "geenEnergie"-state gaan en alle variabelen die in het programma waren opgeslagen blijven ongewijzigd. Wanneer het niveau weer groen wordt kan er zonder problemen verder gedaan worden. 





## Communicatie