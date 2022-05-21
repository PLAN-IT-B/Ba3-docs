---
layout: default
title: Software
parent: De vuilbak
grand_parent: Puzzels
nav_order: 3
---

# Software

[Klik hier voor onze code](https://github.com/PLAN-IT-B/BachelorProefGarbage/tree/main/Code)

- [Verschillende staten](#verschillende-staten)
- [Communicatie](#communicatie)
- [Overige functies](#overige-functies)
- [Flowchart](#flowchart)

## Verschillende staten
Om de code overzichtelijk te maken hebben we gewerkt met verschillende staten. In welke staat de vuilbak zich bevind zal telkens in de loop bekeken worden aan de hand van booleans die veranderen. Deze booleans zijn:
* reset: Moet de ESP gereset worden?
* energie: Is er genoeg energie? Staat de buffer in de groene zone?
* actief: Is de code al eens juist ingegeven?
* checkVuilnisTotaal: Is al het vuilnis al gevonden?
* wachtOpGewicht: Is er al gewicht gedetecteerd?

Werken met deze staten was zeer handig aangezien er hierdoor geen probleem ontstond wanneer het niveau plots niet meer groen was. Het programma zou gewoon naar zijn "geenEnergie"-state gaan en alle variabelen die in het programma waren opgeslagen blijven ongewijzigd. Wanneer het niveau weer groen wordt kan er zonder problemen verder gedaan worden. 

Merk op dat er in vrijwel elke staat gebruikt gemaakt wordt van volgend stuk code:

```c++
if(bl ==false){
      lcd.backlight();
      bl = true;
    }
```
Dit stukje zorgt er voor dat de backlight van de LCD maar 1* wordt opgezet nandat deze door een energietekort uit gaat. Moesten we in de loop altijd lcd.backlight() oproepen krijgen we geflikker van het achtergrondlicht.

### resetPuzzel
Deze staat heeft de hoogste prioriteit. Wanneer het resetsignaal is ontvangen zal de ESP resetten.

### geenEnergie
Wanneer de buffer op rood of oranje staat zal het programma in deze staat komen. De backlight van de LCD zal uit blijven en het programma zal niet meer naar inputs luisteren (buiten de communicatie).
```c++
void geenEnergie(){
lcd.noBacklight();
    bl = false;
    codeTekst = false;
}
```
### enkelEnergie
In deze staat zal het programma enkel luisteren naar input van het toetsenbord. Het programma zal aan de hand van de de inputs van het toetsenbord een array vormen, eventuele typfouten kunnen gedelete worden door de '*'-toets. Wanneer de gebruiken vermoed de juiste code ingevuld te hebben kan deze op '#' drukken. Wanneer er dan 5 cijfers zijn ingegeven zal deze array vergeleken worden met de verkrege code-array van de wristbands en de train-game, als deze overeen komt zal de LCD "Code correct" afbeelden, zal er een succes-sound worden afgespeeld en zal de boolean actief op true worden gezet.

```c++
void enkelEnergie(){

  key = keypad.getKey(); //Vraag de input van het toetsenbord op

  //Tegen flikkeren
    if(bl ==false){
      lcd.backlight();
      bl = true;
    }

    //Check of het beginscherm er op staat, anders zet het er op
    if(codeTekst == false){
    lcd.setCursor(2,0);
    lcd.print("Voer de code in:");
    lcd.setCursor(8,2);
    lcd.print("_____");
    codeTekst = true;
    }

    //Serial.print(key);
    
    //Als de knop wordt ingedrukt
    if(key!=NULL){
      
    //Als # (enter wordt ingedrukt)
    if(key =='#'){
      
      
      if(c ==13){ //De positie is het laatste cijfer
      boolean check = true; //Controleer of de code klopt
      for(int i = 0;i<5;i++){
        if(code[i]!=cinput[i]){
          check = false;
        }
      }

      if(check == true){ //Als de code klopt wordt de puzzel actief
        actief = true;
        client.publish("garbage/status","Garbage code is correct ingegeven");
        client.publish("Wristbands","Stop Wristbands");
        lcd.clear();
        lcd.setCursor(4,1);
        lcd.print("Code correct");
        succesSound();
        delay(500);
        lcd.clear();
        codeTekst = false;

      }
      
      else{ //Bestraffen en reset
        client.publish("TrappenMaar/buffer","grote fout");
        client.publish("TrappenMaar/buffer","grote fout");
        lcd.setCursor(8,2);
        lcd.print("_____");
        c = 8;
        failureSound();

      }
    }

    }

```

### puzzel
Dit is het eigenlijke deel van de puzzel. Deze staat zal beginnen met een simpele uitleg van de puzzel op de LCD te zetten. Hierna gaat hij kijken of een scanknop wordt ingedrukt. Wanneer dit het geval is zal de desbetreffende RFID scanner aangesproken worden, hierover later meer. Ten slotte zal het programma in deze staat checken of al het vuilnis al is gevonden, indien dit het geval is zal de boolean checkVuilnisTotaal op true gezet worden.

```c++
void puzzel(){
   //Tegen flikkeren
    if(bl ==false){
      lcd.backlight();
      bl = true;
    }

    //Test scale
   // Serial.println(scale.get_units(), 3);

    //Serial.println("In puzzel");

    if (codeTekst == false){ //LCD instellen
    lcd.setCursor(0,0);
    lcd.print("Zoek vuilnis, scan");
    lcd.setCursor(0,1);
    lcd.print("in het juiste vak");
    lcd.setCursor(0,2);
    lcd.print("en gooi het in de");
    lcd.setCursor(0,3);
    lcd.print("correcte vuilnisbak");
    codeTekst = true;
    }

  //Check of knoppen zijn ingedrukt en lees de juiste scanner
  if(digitalRead(Button_pin1) == HIGH){
    scanRFID1();
  }
  if(digitalRead(Button_pin2) == HIGH){
    scanRFID2();
  }
  if(digitalRead(Button_pin3) == HIGH){
    scanRFID3();
  }

  if(rest == 3 && pmd == 3 && p_k == 4){ //Is al het vuilnis gevonden?
      checkVuilnisTotaal = true;

 }}
```
### gewichtWachter
Deze staat zal er voor zorgen dat het programma niet verder gaat totdat het afval na het scannen in de juiste vuilnisvak geworpen wordt. In de functie waar de RFID-scanner wordt opgeroepen zal indien een juist stuk afval gescand wordt een weegschaalnummer meegegeven worden. Op deze weegschaal wal een gewichtsverschil moeten wijn van minstens 10g voordat de puzzel weer verder kan gaan.
```c++
void gewichtWachter(){ 
    if (codeTekst == false){
    lcd.clear();
    lcd.setCursor(2,1);
    lcd.print("Gooi het vuilnis");
    lcd.setCursor(2,2);
    lcd.print("in de juiste bak");
    codeTekst =true;

    switch(nummerWeegschaal){ //Oorsprongkelijk gewicht
      case 1: vorigGewicht = scale.get_units(); 
      Serial.println(vorigGewicht);
      break;

      case 2: vorigGewicht = scale2.get_units(); 
      Serial.println(vorigGewicht);
      break;

      case 3: vorigGewicht = scale3.get_units(); 
      Serial.println(vorigGewicht);
      break;
    }
  }

    switch(nummerWeegschaal){ //nieuw gewicht
      case 1: huidigGewicht = scale.get_units(); 
      Serial.println(huidigGewicht);
      break;

      case 2: huidigGewicht = scale2.get_units(); 
      Serial.println(huidigGewicht);
      break;

      case 3: huidigGewicht = scale3.get_units(); 
      Serial.println(huidigGewicht);
      break;
    }

      if((huidigGewicht - vorigGewicht)>0.01){ //Is er meer dan 10g bij gekomen?
        //Verlaat deze staat
        wachtOpGewicht = false;
        codeTekst= false;
      }
    }
```



### eindePuzzel
Wanneer al het vuilnis in de vuilnisbak is zal deze staat activeren. Het totale gewicht van alle weegschalen zal slechts 1x genomen worden en zal zo op het LCD scherm worden geprint. De som van deze gewichten is de code die naar het UV-licht zal gestuurd worden. De puzzel is hier op zijn einde. Wanneer er niet genoeg energie is zal het scherm nog uit gaan maar op een reset na is er niets meer mogelijk om de eindcode en het eindbeeld te veranderen.
```c++
void eindePuzzel(){

  //Tegen flikkeren
    if(bl ==false){
      lcd.backlight();
      bl = true;
    }

if (!defGewicht){ //1 maal het gewicht bepalen

//Lees gewicht

  restG = scale.get_units();
  pmdG = scale2.get_units();
  p_kG = scale3.get_units();
 
 //Maak het gewicht een int in gram
  Rrest = round(restG*1000);
  Rpmd = round(pmdG*1000);
  Rp_k = round(p_kG*1000);

 int eindcodeInt = (Rrest+pmdG+p_kG);
 String eindcodeString;

//Zorg dat er altijd 4 cijfers doorgestuurd worden
 if(eindcodeInt <999){ 
 eindcodeString = "0" + String(eindcodeInt,DEC);
 }
 else if(eindcodeInt <99){

   eindcodeString = "00" + String(eindcodeInt,DEC);
 }
 else{
   eindcodeString = String(eindcodeInt,DEC);
 }
 Serial.print(eindcodeString);

  const char* eindcode=eindcodeString.c_str();
  client.publish("garbage/eindcode",eindcode); 


  defGewicht = true;

} 

//print gewicht
if(codeTekst==false){
lcd.clear();
lcd.setCursor(2, 0);
lcd.print("Alles gesorteerd");
lcd.setCursor(1, 1);
lcd.print("Hoeveel in totaal?");
lcd.setCursor(0, 2);
lcd.print("PMD     Rest     P&K"); 
lcd.setCursor(3,3);
lcd.print("g");
lcd.setCursor(12,3);
lcd.print("g");
lcd.setCursor(19,3);
lcd.print("g");
codeTekst = true;

lcd.setCursor(0,3);
lcd.print(Rrest);
  
lcd.setCursor(9,3);
lcd.print(Rpmd);
  
lcd.setCursor(16,3);
lcd.print(Rp_k);
}

}
```

## Communicatie
Zoals besproken in communicatie verbinden we met de MQTT server. We subscriben in de code naar volgende mappen:
* wristbands/3cijfers (initieel zouden hier 3 cijfers in doorgestuurd worden, pas later werden dit er 4.)
* treingame/#
* controlpanel/reset
* TrappenMaar/zone

In de callback functie luisteren we naar verschillende boodschappen:
* Reset escaperoom: reset zal op true gezet worden en de ESP wordt gereset.
* groen, oranje of rood: Aan de hand hiervan zal de variabele "energie" gecontroleerd worden.
* Wristband-code .... : Wanneer "Wristband-code" in een bericht wordt ontvangen zal character 15-18 in de code array gezet worden. Dit komt overeen met de 4 cijfers die worden doorgestuurd.
* Trein-code .: Wanneer "Trein-code" in een bericht wordt ontvangen zal character 11 in de code array geplaatst worden.

## Overige functies

### scanRFID
Deze functie wordt opgeroepen wanneer het programma in de puzzel-state is en een scan knop ingedrukt wordt. Eerst wordt de multiplexer ingesteld om te sturen naar de correcte scanner, dan wordt er heb het scherm "Scanning ..." geschreven en daarna wordt er effectief gescand. De scanner zal een halve seconde zoeken achter een tag, wanneer er niets gescand wordt zal de functie beëindigd worden. 
Als er wel iets gescand wordt zal er gecontroleerd worden of het ID van de tag voorkomt in de rij van tags die bij deze scanner (soort vuilnis) horen. Als dit het geval is zal in de rij deze tag aangepast worden naar [0,0,0,0,0,0,0,0], hierdoor kan deze tag niet 2x gescand worden (In de gebruikelijke werking van de escape room maakt dit niet uit maar in een alternatieve versie waar je energie krijgt voor het correct sorteren wel). Ook zal er een succes geluid afgespeeld worden en zal het programma naar de gewichtWachter-staat gaan. 
Wanneer de tag fout is zal er een failure sound afgaan en zal er "kleine fout" naar de buffer gestuurd worden.

### Sound
Doordat we gebruik maken van het "Tone32" library konden we noten afspelen via een GPIO pin. Omdat de speaker niet zo luid ging en we controle wouden hebben over het volume hebben we er een versterker tussen geplaatst. 

### TCA9548A
Deze functie zal naar de multiplexer sturen welk kanaal moet aangesproken worden

## Flowchart

![](FlowCode.png)
