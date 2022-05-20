---
layout: default
title: Fiets
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 3
---
# Fiets
## Algemeen

Het spel valt en staat met een hometrainer waarvan de snelheid waaraan een speler fietst met een dynamometer wordt opmeten. De gemeten AC-spanning uit die dynamometer wordt met een bruggelijkrichter omgezet naar een gelijkspanning. Met behulp van een condensator wordt die gelijkgerichte spanning omgezet naar een
constante gelijkspanning welke via de functie analogRead(PinNummer) kan worden uitgelezen met de ESP32.
Door hier te kiezen voor een condensator met grote capaciteitswaarde (1000μF), kan het gefietste niveau makkelijker constant
gehouden worden aangezien de ontlaadtijd van deze condensator dan groter is (τ = R x C).   

Met behulp van 3 blauwe leds die één voor één gaan branden, komt de fietser te weten hoeveel tijd
hij nog heeft om het correcte fietsniveau te bereiken. Via een RGB-led die groen of rood wordt, wordt getoond of het gefietste niveau correct was. De huidige fietssnelheid wordt visueel getoond a.d.h.v. een LCD (20x4) met i<sup>2</sup>C aansluiting.   

De energie die gegenereerd wordt door het fietsen, wordt tenslotte ook gebruikt om een UV-lamp aan te sturen die nodig is bij de laatste puzzel.

## KiCad project

[Visit our Github to find our Kicad project!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/KiCad/Kicad-versie%20Bert/Fiets%20-%20zelfgemaakte%20level%20shifter/Fiets%20-%20zelfgemaakte%20level%20shifter)

### EEschema
![](2022-05-13-21-32-56.png)
### PCB ontwerp
![](2022-05-13-21-33-13.png)
![](2022-05-13-21-33-27.png)
### Aansluitingen en onderdelen
Aan de fiets hangt een 20x4 LCD, 3 blauwe leds en 1 RGB led. Voor de LCD gebruiken we een level shifter die ervoor zorgt dat de SDA en SCL lijnen die op 5V staan aan de kant van de LCD op een 3.3V niveau gezet worden vooraleer ze respectievelijk de GPIO21 en GPIO22 pinnen van onze ESP binnenkomen. De kathode van de 4 leds worden met een gezamelijke ground verbonden en de anode's met verschillende GPIO-pinnen van onze esp.
### voedinsspanning
Aangezien hier gebruik wordt gemaakt van een LCD die een voedingsspanning van 5V nodig heeft, is het het eenvoudigste om gebruik te maken van
een powerbank om de esp te voeden. Deze 5V wordt namelijk rechtstreeks als voeding gebruikt voor de LCD en voedt m.b.v. een LDO de ESP32 die 3.3V nodig heeft.

## Code
De code van de fiets bestaat vooral uit het opmeten van het DC signaal (dat door middel van een spanningsdeler maximaal 3.3 volt zal zijn).
```c
  // read the input on analog pin 34:
  int sensorValue = analogRead(34);
  // Convert the analog reading (which goes from 0 - 4095) 
  //to a voltage (0 - 3.3V):
  float voltage = sensorValue * (3.3 / 4095.0);
  Serial.print(sensorValue);
  Serial.print(" ");
  // print out the value you read:
  Serial.print(voltage);
  Serial.print(" ");
  Serial.println(100*voltage);
  display(100*voltage);
```
Wij gebruiken hier dus pin 34 die de spanning, opgewekt door de dynamo en daarna omgezet werd naar een DC, zal opmeten. Aangezien de esp32 de waarde op deze analoge pin omzet naar een digitale waarde met behulp van een ADC, kunnen we hier een waarde tussen 0 en 4095 uitkomen. We zetten hier dus sensorValue om naar de effectieve 'voltage'. 
Met behulp van de methode 'display' wordt deze waarde omgezet naar een bepaald level tussen 1 en 4 dat visueel wordt weergeven op de LCD. 

------- FOTO TOEVOEGEN LCD --------------

Met behulp van een simpele if-else if structuur wordt een bepaald deel van het scherm zwart gekleurd. Op die manier krijgen we een eenvoudige snelheidmeter.   

Deze code bestaat vooral nog uit een 'control' functie die vanuit de loop opgeroepen wordt telkens er een nieuw bericht van buitenaf toekomt. Zo luistert de fiets naar de volgende berichten en voert daarna volgende acties uit: 
* send: wanneer een 7 segment 5 seconden gebrand heeft, moet de fiets de op dat moment gefietste snelheid doorsturen naar de buffer die dan controleert of de speler aan het correcte tempo aan het fietsen was. 
* led1, led2 en led3: de fiets zal één voor één drie ledjes laten branden. Deze laten de speler weten hoelang nog te hebben alvorens het 7-segment display uit gaat en ze dus de correcte snelheid moeten bereikt hebben. 
* correct/false: wanneer de speler correct/foutief gefietst heeft, wordt een groene/rode led aangezet waarna die 1 seconde brandt en daarna samen met led1, led2 en led3 uitgezet wordt. Hierna wordt naar de buffer "newNumber" gestuurd waarna deze een nieuw seven-segment display de opdracht zal geven een nummer weer te geven. 
* resetFiets: nu wordt de fiets gereset. Hierna stuurt die naar de buffer dat het resetten voltooid is. De buffer moet namelijk als laatste opgestart/gereset worden om een correcte werking van het spel te verzekeren.
    
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/MeasuringDcVoltageWithCommunicationBuffer)


## realisatie
FOTO TOEVOEGEN