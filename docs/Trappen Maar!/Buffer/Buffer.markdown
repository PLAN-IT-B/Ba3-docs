---
layout: default
title: Buffer
parent: Trappen Maar!
grand_parent: Puzzels
nav_order: 3
---
# Buffer
## Algemeen
De buffer bestaat uit 16 ledstrips waarvan de onderste 5 rood zijn, de middelste 6 oranje en de bovenste 5
groen. Afhankelijk van de score van de spelers gaat er een ander aantal ledstrips branden en komt het spel in
een bepaalde kleurenzone (rood, oranje of groen) terecht. De kleurenzone bepaalt welke andere escaperoom spelletjes
gespeeld kunnen worden/ welke spelletjes geblokkeerd zijn.

## KiCad project
### Elektrisch schema
![](2022-05-13-21-31-35.png)
### PCB ontwerp
![](2022-05-13-21-31-51.png)
![](2022-05-13-21-32-12.png)
### Aansluitingen en onderdelen
De buffer heeft heel wat aansluitingen die nodig zijn om de 16 energieniveaus individueel te kunnen bedienen. Gezien deze is opgebouwd uit ledstrips die een spanning van 12 volt vereisen, was het niet mogelijk deze rechtstreekts te voeden via de esp. Het was dan ook nodig om te werken met een externe voedingsbron die de ledstrips rechtstreeks voedt. Om toch de ledstrips aan- en uit te kunnen schakelen, was het aangewezen te werken met MOSFETs (hier type 2N7002). De gate van die componenten kan dan rechtstreeks verbonden worden met een unieke GPIO-pin van de esp. Vervolgens is het een kwestie van drain en source te verbinden met een R, G of B pin respectievelijk ground. Zo zullen deze N-type MOSFET's een gesloten lus vormen naar ground en de ledstrip laten branden wanneer die GPIO-pin hoog staat. Op deze manier kunnen de kleuren rood en groen eenvoudig gerealiseerd worden. Om geel/oranje te bekomen, moet er een extra MOSFET en GPIO-pin gebruikt worden die een duty-cycle genereren. Wanneer alle G-aansluitingen van de ledstrips in zone oranje via hun eigen MOSFET op deze duty-cycle-MOSFET worden aangesloten, zal groen heel snel knipperen. Op deze manier kan, in combinatie met het wel constant branden (wanneer gewenst) van de rode LEDs op die strips, oranje/geel licht gecreëerd worden.

### voedinsspanning
Aangezien we hier met ledstrips werken zijn we genoodzaakt gebruik te maken van 12V. We werken hier
dus met een 12V pc voeding die rechtstreeks onze ledstrips voedt en die via een buck-converter en LDO
(ingebouwd in onze PCB) onze ESP voedt met 3V3.
## Code
De code van de buffer is in de link hieronder terug te vinden. Aangezien deze het centrale element is in de escaperoom, zal hij heel wat verschillende taken hebben. Zo zal hij bij het binnenkomen van een nieuw bericht, vanuit de loop() de functie 'control' oproepen. Afhankelijk van het bericht zal hij volgende zaken doen: 
* newNumber: de buffer zal nu via de methode 'int randomGetal = random(1,5)' een random getal genereren tussen 1 en 4 en dit sturen naar een random esp. Dit laatste doet hij ook via de functie 'random(1,X-1)' met X het aantal 7 segments dat je gebruikt. Via een functie 'sentTo7Segment' wordt nu naar het correct 7 segment dit willekeurige getal gezonden. 
* '0', '1', '2', '3', '4': deze berichten staan voor de verschillende levels die door de speler gefietst kunnen worden. Dit gefietse level worden dan in de methode 'checkmessage' vergeleken met het getoonde nummer op het 7 segment. Vanuit deze methode wordt dan het bericht 'correct' of 'false' gestuurd naar de fiets (zie uitleg code fiets) en wordt bij het correct fietsen de bufferValue met een bepaalde waarde (afhankelijk van de moeijlijkheid van het spel) verhoogd. We zorgen er hier wel voor dat ze geen hogere score kunnen behalen dan de maximale score van onze buffer. Ook deze kunnen variabel, naargelang de moeilijkheid van het spel, worden aangepast. 
* grote fout, kleine fout: andere escaperoom spelletjes kunnen strafpunten sturen die afgetrokken worden van de buffer indien de speler daar een fout maakt. Hoeveel punten er effectief van de buffer afgetrokken worden kan vrij gekozen worden.
* reset: bij het opstarten van de escapreroom wordt naar alle puzzles de boodschap reset gestuurd. Bij deze puzzle is het belangrijk dat eerst de fiets en 7 segmenten resetten voordat de buffer reset. Daarom wordt eerst vanuit deze methode 'resetFiets', 'resetSegment1', resetSegment2', ... gestuurd naar de andere esp's van onze puzzle.
* Fiets ready, Segment1 ready, ... : Wanneer nu de fiest en seven segmenten allemaal gereset zijn zal de buffer zelf gaan resetten. 

In onze code worden bepaalde fouten, zoals een verloren bericht en connectie, opgevangen. Wanneer de buffer namelijk na 15 seconden nog geen nieuw bericht ”newNumber” gekregen heeft zou het kunnen dat een seven segment niet meer werkt/het bericht verloren is gegaan en indien dit het geval is, produceert de buffer zelf een nieuw random getal naar een willekeurig seven segment. Dit gebeurt via volgende code: 
```c
 if (millis() - startTime >= 15000) {
      // 5 seconds have elapsed. ... do something interesting ...
      if(oldControlSend == controlSend){

          Serial.println("newNumber werd OPNIEUW gestuurd naar buffer");   
          int randomGetal = random(1,5);
    
          itoa(randomGetal,cstr,10);
          //OORSPRONKELIJK TUSSEN 1 EN 5
          int randomEsp = random(1,5); //getal 1,2, 3 of 4
          Serial.println("randomESP");
          Serial.print("esp: ");
          Serial.print(randomEsp);
          Serial.print("\tnummer: ");
          Serial.print(randomGetal);
          sendTo7Segment(randomEsp, cstr);
      }
          oldControlSend = controlSend;
          startTime = millis();
   }
```
 Om dit te realiseren maken we dus gebruik van de functie millis(). Dit is een soort counter die bij het opstarten van dit programma is beginnen lopen. Door hier telkens startTime, na het doorlopen van deze if-lus, opnieuw gelijk te stellen aan de huidige millis(), kunnen we als voorwaarde voor onze if-lus  volgende code gebruiken om te controleren of er 15 seconden voorbij zijn: 'millis() - startTime >= 15000'. ControlSend is een variabele die telkens een nieuw bericht "newNumber" verhoogt wordt. Door zijn waarde dan te vergelijken met "oldControlSend" kunnen we controleren of er in de voorbije 15 seconden een bericht is toegekomen.    

 Elke loop wordt hier ook de methode 'setBuffer' opgeroepen. Deze zal naargelang de score van de spelers een bepaald aantal ledstrip laten branden. We doen dit simpelweg via de functie 'digitalWrite' waarmee we elke ledstrip apart aan of uit kunnen zetten.
 We werken hier met een variabele 'Maxscore' die gelijk gesteld wordt aan '16*interval'. Er kan hier zelf een waarde voor 'interval' gekozen worden naargelang de moeilijkheid van het spel.  

 Als laatste dient de buffer naar de andere escaperoom-games ook nog de zone door te sturen van het energieniveau van de buffer. We kozen ervoor enkel een bericht te sturen wanneer de zonekleur verandert. De code hiervoor is de volgende: 
 ```c
if(oldColor != color){
    if(color == "rood"){
      client.publish("TrappenMaar/zone", "rood");
    }
    else if(color == "oranje"){
      client.publish("TrappenMaar/zone", "oranje");

    }
    else if(color == "groen"){
      client.publish("TrappenMaar/zone", "groen");
    }
    else if(color = "niets"){
      client.publish("TrappenMaar/zone", "niets");
    }
     color = oldColor;
}
 ```
 
[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefTrappenMaar/tree/main/Volledige%20en%20werkende%20code/sender)
## realisatie
FOTO TOEVOEGEN