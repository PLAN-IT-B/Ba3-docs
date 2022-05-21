---
layout: default
title: Software
parent: Train Game
grand_parent: Puzzels
nav_order: 2
---

# Software
Om de code voor onze bachelorproef overzichtelijk te houden hebben we ervoor gekozen om per onderdeel een apparte c++ file te voorzien. Elk stuk hardware werd eerst grondig getest los van elkaar zodat we stap voor stap tewerk konden gaan. elke C++ file bestaat uit een klasse met enkele functies. Deze functies worden nadeien opgeroepen in de main. Dit heeft ervoor gezorgd dat we een overzicht konden houden over de software.

## Main
Het hart van onze software is de main. De belangrijkste delen hiervan zijn de setup() en loop(). De setup maakt mogelijk op bepaalde functies uit te voeren die slechts één keer doorlopen moeten worden helemaal aan het begin van de code. De loop wordt uitgevoert net na de setup. Deze zal telkens weer uit gevoerd worden wanneer we aan het einde komen van deze loop.

### setup
In de setup gaan we alle code uitvoeren die moet worden uitgevoerd worden voordat de loop begint. In de realiteit betekend dit begintoestand van de LEDS en LCD initialiseren, verbinding met de MQTT broker tot stand brengen en de pinmode van de joystick instellen.  
```c
void setup() {
  //Baudrate
  Serial.begin(115200);

  //Comunicatie
  setup_wifi();
  client.setServer(MQTT_SERVER, MQTT_PORT);
  client.setCallback(callback);
  
  //LCD
  tft.init();
  tft.setRotation(3);
  tft.fillScreen(TFT_WHITE);
  tft.setTextColor(TFT_BLACK,TFT_WHITE);   

  //JOYSTICK
  pinMode(34,INPUT);    //x
  pinMode(35,INPUT);    //y

  //LEDS
  l1->resetLeds();
}
```
### loop
De loop is opgedeeld in verschillende delen. De communicatie en de 3 apparte games.
Bij de communicatie gaan we iedere keer nagaan of we nog ge connecteerd zijn met de MQTT broker. Als dit het geval is gaan we de reconnect functie uitvoeren om de verbinding te herstellen.

!!!

```c
//Comunicatie
   if (!client.connected())
  {reconnect();}

  client.loop();
  long now = millis();

  if (now - lastMsg > 5000)
  {
    lastMsg = now;
  }
```
Hieronder is er een voorbeeld gegeven van één van de drie games. Om ervoor te zorgen dat de juiste puzzel actief is in de loop hebben we de bool deel1, deel2 en deel3 gemaakt. Op elk moment is er maar één deel actief. De effectieve werking van de game zelf wordt hieronder nog besproken bij het onderdeel TrainTrace.

Ter verduidelijking van de code omtrent het gameverloop is er een flowchart gemaakt. De legende maakt duidelijk over welke code file het gaat.

![Zones Joystick](Software_Flowchart.png)
```c
// Ticket
  if(deel1){
    //bewegen van de trein
    t2->volgendeStad(zone);
    //Deze code wordt enkel de eerste keer uitgevoerd.
    //Dit is zoals de setup aan het begin van de code.
    if(tt1_once){
      t2->setStad(4);
      tt1_once = false;
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,ticketWidth,ticketHeight,ticket);
    }
    //Controleren of de code correct uitgevoerd is.
    tt1->TicketCheckTrace(t2->getStad());
    Serial.print(tt1->getTicketCheckcomplete());
    Serial.println();
    //Code wanneer er een fout gemaakt wordt.
    if(!tt1->ticketGetAllesKits()){
      t2->setStad(tt1->ticketGetBegin());
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,wrongWidth,wrongHeight,wrong);
      l1->reset();
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,ticketWidth,ticketHeight,ticket);
      tt1->ticketSetAllesKits(true);
      client.publish("TrappenMaar/buffer","kleine fout");
    }
    //Code wanneer de puzzel juist is opgelost.
    if(tt1->getTicketCheckcomplete()){
      Serial.println("TICKET COMPLEET!!!");
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,correctWidth,correctHeight,correct);
      l1->completed();
      codeDeel1 = randomGetal;
      tft.fillScreen(TFT_WHITE);
      tft.drawCentreString("Het laatste cijfer is:",160,50,4);
      tft.drawFloat(codeDeel1,0,130,100,8);
      String CodeString = "Trein-code " + String(codeDeel1,DEC);
      const char* CodeChar = CodeString.c_str();
      //code sturen naar garbage
      client.publish("treingame/4decijfer",CodeChar); 
      Serial.println("Trein-code " + String(codeDeel1,DEC));
      //
      deel1=false;
      deel2=true;
    } 
  }
```
Wanneer alle games opgelost zijn wordt er een loopledje over het hele bord weergegeven. Dit heeft als enige betekenis om aan te tonen dat de laatste puzzel is opgelost en dat er geen aandacht meer moet worden besteed aan dit deel van de escaproom. 
```c
// Alle delen compleet
  if (!deel1&&!deel2&&!deel3){
    l1->loopleds();
    tft.drawCentreString("Do UV what I see?",160,100,4);
  }
```


## Joystick

In de Joystick file gaan we de 2 analoge inputs X en Y een betekenisvolle vorm geven. We delen het bereikbare vlak van X- en Y-waarden op in zones. De zones gebruiken we later om te beslissen naar welke stad de trein zal rijd. Hiervoor zijn de parameters onder, boven, w1, w2, w3 en w4 aangemaakt. Deze kunnen aangepast worden zodanig dat de zones eenvoudig veranderd kunnen worden in grootte. De "deadzone" is de rusttoestand, hierbij zal de trein niet bewegen.

![Zones Joystick](Joystick_cut.png)
```c
#include "Drukknop.h"
#include <math.h>
#define onder   500
#define boven   3595
#define w1      819
#define w2      1638
#define w3      2457
#define w4      3276

//Aan de hand van de x en y waarden van de joystick bepalen we de richting waarin de joystick stuurt
int Drukknop::bepaalZone(int x, int y){
    int i=0;                                   
    if(x<=onder && w4<=y)               {i=7;} 
    if(x<=onder && y<w4 && w3<=y)       {i=8;} 
    if(x<=onder && y<w3 && w2<=y)       {i=9;} 
    if(x<=onder && y<w2 && w1<=y)       {i=10;} 
    if(x<=onder && y<w1)                {i=11;} 

    if(boven<=x && w4<=y)               {i=3;}
    if(boven<=x && w3<=y && y<w4)       {i=2;}
    if(boven<=x && w2<=y && y<w3)       {i=1;}
    if(boven<=x && w1<=y && y<w2)       {i=16;}
    if(boven<=x && y<w1)                {i=15;}

    if(y<=onder && w4<=x)               {i=15;} 
    if(y<=onder && x<w4 && w3<=x)       {i=14;} 
    if(y<=onder && x<w3 && w2<=x)       {i=13;} 
    if(y<=onder && x<w2 && w1<=x)       {i=12;} 
    if(y<=onder && x<w1)                {i=11;} 

    if(boven<=y && w4<=x)               {i=3;}
    if(boven<=y && w3<=x && x<w4)       {i=4;}
    if(boven<=y && w2<=x && x<w3)       {i=5;}
    if(boven<=y && w1<=x && x<w2)       {i=6;}
    if(boven<=y && x<w1)                {i=7;}

    return i;
}


//We bepalen de deadzone, deze inputwaarden van de joystick willen we negeren
bool Drukknop::deadZone(int x, int y){
    bool dead =false;
    if(onder<x && x<boven && onder<y && y<boven){
      dead=true;}
    return dead;
}
```
## LEDS
Aan de hand van deze file gaan we functies opbouwen die we gebruiken om de LEDS aan te sturen. Eerst en vooral defigneren we de kleuren die we gebruiken.
```c
#include "LEDS.h"

//We maken gebruik van de NeoPixel library
Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);


//Declaratie van verschillende kleuren.
uint32_t oranje = strip.Color(140, 255, 0);
uint32_t groen = strip.Color(255, 0, 0);
uint32_t rood = strip.Color(0, 255, 0);
uint32_t treinkleur = strip.Color(255, 0, 255);
uint32_t uit = strip.Color(0, 0, 0);


//Alle leds uit.
void LEDS::resetLeds(){
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
}
```
LoopVanXNaarY is de functie die een aantal opeenvolgende leds zal doen oplichten. Dit is de functie die de trein doet bewegen van stad X naar stad Y. Hierbij moet enkel de begin- en eindLED meegegeven worden.
```c
//Door de positionering van de LEDS hebben we twee stukken code geschreven 
//om de leds in de juiste volgorde te laten branden.

//Hieronder volgt de algemene code. Deze gebruiken we voor de trajecten
//waarbij de LEDS mooi op volgorde zitten tussen de steden. We hoeven geen 
//specifiek volgordes te programmeren.
void LEDS::loopVanXNaarY(int x, int y, int stad){
  if(x<y){
    for(int i=x; i < y+1; i++) {
    strip.setPixelColor(i-1, uit);
    strip.setPixelColor(i, treinkleur);
    strip.show();
    delay(200);
     }
  }
  if(x>y){
    for(int i=x; i > y-1; i--) {
    strip.setPixelColor(i+1, uit);
    strip.setPixelColor(i, treinkleur);
    strip.show();
    delay(200);
     }
  }
  strip.setPixelColor(y, uit);
  delay(100);
  strip.show();
}
```
Niet in elk geval is het mogelijk om tussen 2 steden bovenstaande functie te gebruiken. Die functie kan enkel gebruikt worden wanneer alle leds in volgorde staan, hieronder kan je een voorbeeld vinden. De oplossing hiervoor is om de route hiertussen Led per Led te bepalen.
```c
//waarbij de LEDS niet op volgorde zitten tussen de steden. 
void LEDS::AntwerpenBrussel(){
    
    strip.setPixelColor(37, treinkleur);
    strip.show();
    delay(200);
    strip.setPixelColor(37, uit);
    strip.setPixelColor(28, treinkleur);
    strip.show();
    delay(200);
    strip.setPixelColor(28, uit);
    strip.setPixelColor(27, treinkleur);
    strip.show();
    delay(200);
    strip.setPixelColor(27, uit);
    strip.show();
}
```
Wanneer we in een stad aankomen moeten we ook een functie hebben om aan te geven in welke stad de trein aan het wachten is. Dit indikeren we door die led te doen "blinken". Om de code overzichtelijk te houden geven we de stadsnummer mee. Aan de hand van een switch gaan we de juiste stad doen "blinken".
```c
//LED van stad laten knipperen.
void LEDS::blinkStad(int stadNr, int aantal, int wacht){
    switch (stadNr)
    {
    case 1:                         //BRUGGE
        for(int i=0; i<aantal;i++){
            strip.setPixelColor(5, treinkleur);
            strip.show();
            delay(wacht);
            strip.setPixelColor(5, uit);
            strip.show();
            delay(wacht);
        }
        strip.setPixelColor(5,oranje);
        strip.show();
        break;
    }

    ...
}
```
Aan het eind van de hele puzzel en wanneer de volgende puzzel gespeeld kan worden gaan we alle leds van 0 tot 99 aflopen om de aandacht te lokken van de spelers.
```c
//Alle leds laten doorlopen (op het einde van de game)
void LEDS::loopleds() {
    for (int i=0;i<100;i++){
        strip.setPixelColor(i, 255,255,255);
        strip.setPixelColor(i-1, 0,0,0);
        strip.show();
        delay(20);
    }
    for(int j=0; j<2;j++){
        strip.fill( oranje, 0, 100);
        strip.show();
        delay(100);
        strip.fill( uit, 0, 100);
        strip.show();
        delay(100);
    }
}
```

```c
//We laten desteden rood oplichten om aan te tonen dat er een 
//foute handeling werd uitgevoerd door de spelers.
void LEDS::reset(){
    for(int i=0; i<5;i++){
        strip.setPixelColor(0, rood);
        strip.setPixelColor(5, rood);
        strip.setPixelColor(9, rood);
        strip.setPixelColor(21, rood);
        strip.setPixelColor(26, rood);
        strip.setPixelColor(38, rood);
        strip.setPixelColor(45, rood);
        strip.setPixelColor(73, rood);
        strip.setPixelColor(79, rood);
        strip.setPixelColor(87, rood);
        strip.setPixelColor(93, rood);
        strip.show();
        delay(100);
        strip.setPixelColor(0, uit);
        strip.setPixelColor(5, uit);
        strip.setPixelColor(9, uit);
        strip.setPixelColor(21, uit);
        strip.setPixelColor(26, uit);
        strip.setPixelColor(38, uit);
        strip.setPixelColor(45, uit);
        strip.setPixelColor(73, uit);
        strip.setPixelColor(79, uit);
        strip.setPixelColor(87, uit);
        strip.setPixelColor(93, uit);
        strip.show();
        delay(100);
    }
}
```
```c
//Alle steden laten branden tijdens normaal verloop van de game.
void LEDS::showSteden(){
    strip.setPixelColor(0, oranje);
    strip.setPixelColor(5, oranje);
    strip.setPixelColor(9, oranje);
    strip.setPixelColor(21, oranje);
    strip.setPixelColor(26, oranje);
    strip.setPixelColor(38, oranje);
    strip.setPixelColor(45, oranje);
    strip.setPixelColor(73, oranje);
    strip.setPixelColor(79, oranje);
    strip.setPixelColor(87, oranje);
    strip.setPixelColor(93, oranje);
    strip.show();
}
```

## Trein
In Trein.h wordt de besturing van de trein geregeld. Je kan een trein toevoegen aan het spel door er een beginstad aan mee te geven. Vanaf we een trein hebben kunnen we die verplaatsen door de functie volgendeStad() uit te voeren.

VolgendeStad() gaat de zone die gemaakt is in Drukknop.h gebruiken voor de bepaling van de volgende stad. Aan de hand van een switch gaan we naar het juiste deeltje code binnen deze code. Met behulp van de kaart en de zones hebben we bepaald welke zones naar welke stad wijzen. Als de juiste zone actief is gaan we de stad van de trein verplaatsen en de trein doen lopen naar de volgende stad m.b.v. LEDS.h. Indien geen enkele zone overeen komt met een volgende stad gaan we de huidige stad doen "blinken".
```c
#include <Trein.h>

LEDS* l2;

//We gebruiken integers om de steden voor te stellen
//Beginstad vastleggen
Trein::Trein(int start){
    this->stad = start;
}

//Bepalen van de de volgende stad afh. van de huidige stad 
//en de input van de joystick
void Trein::volgendeStad(int zone){
    switch(this->stad) {
  case 1:   // Van Brugge
    if(zone==1 || zone==2 || zone==15 || zone==16) {    //Naar Gent
      this->setStad(3); 
      l2->loopVanXNaarY(6,8,stad);
      break;}
    if(zone==10 || zone==11 || zone==12 || zone==13) {  //Naar Kortrijk
      this->setStad(4);  
      l2->loopVanXNaarY(4,1,stad);
      break;}
    else l2->blinkStad(1,1,100); 
    break;

    }
}
```
## TrainTrace
Nu we een werkende Trein hebben kunnen we beginnen met de games zelf te creëren. We gaan voorafaan de routes al bepalen. Dit doen we door een array bij te houden die de steden die gepasseerd moeten worden in volgorde zal bijhouden.
```c
private:
    //Ticket
        int ticket[9] = {7,8,5,9,3,4,6,9,2};
        bool ticketAllesKits = true;
        int ticketTeller = 0;
        int ticketBeginstad = 7;
        bool ticketCheckComplete = false;
        int ticketVergelijkStad = 7;
```
We hebben nu een manier nodig om te bepalen of we nog steeds het correcte traject aan het volgen zijn. Als de trein zich verplaatst naar een volgende stad gaan we bekijken of die gelijk is aan de volgende stad in de array. Indien dit niet het geval is gaan we de fout doorgeven door AllesKits=false te plaatsen en de trein weer in zijn beginpositie te plaatsen. Als de trein toch juist aan het rijden is controleren we of we al bij de laatste stad is aangekomen. Dit signaal gebruiken we om mee te geven dat dit spel is afgerond. 
```c
//We controleren of het juiste traject werd afgelegd.
//We houden in een array ticket[] de juiste volgorde bij.
//Op het einde wordt aan de hand van ticketCheckComplete aangegeven dat de game correct werd doorlopen.
void TrainTrace::TicketCheckTrace(int stad){
    if(stad != ticketVergelijkStad){
        this->ticketTeller++;
        ticketVergelijkStad = stad;
        
        if(stad != ticket[ticketTeller]){
            this->ticketAllesKits=false;
            this->ticketTeller=0;
            this->ticketVergelijkStad = ticketBeginstad;
        }
        if(ticketTeller==8){
            this->ticketCheckComplete=true;
        }
    }
    Serial.print(ticketAllesKits);
    Serial.print(ticketTeller);
}


//Getters en setters
bool TrainTrace::getTicketCheckcomplete(){
    return ticketCheckComplete;
}

bool TrainTrace::ticketGetAllesKits(){
    return ticketAllesKits;
}

void TrainTrace::ticketSetAllesKits(bool i){
    this->ticketAllesKits = i;
}

int TrainTrace::ticketGetBegin(){
    return ticketBeginstad;
}
```
Voor het tweede spel 'Traintrace' te spelen kiezen we random uit 10 routes die we op voorhand gemaakt hebben. Dit doen we om variëteit te brengen in de routes. Dit doen we door een random getal van 0 tot 9 te laten beslissen welke route we gebruiken bij dit spel. Dit doen we 3 maal na elkaar. Als laatste hebben we nog een functie die de route zal weergeven door de steden in de trace één voor één te doen oplichten. We geven een parameter speed mee om een moeilijkheidsgraad mee te geven.  
```c
void TrainTrace::trace1randomroute(int rand){
    switch (rand){   
    case 0: {                  
        int X10[8]={6,7,11,10,6,4,1,3};
        for (int i=0; i<8; i++){
        trace3[i] = X10[i];}
        this->trace3Beginstad=6;
        this->trace3VergelijkStad=6;
        break;}

        ...
    }
}

void TrainTrace::trace3Voorbeeld(int speed){
    for(int i=0; i<AANTAL; i++){
        l3->blinkStad(trace3[i], 2, 20);
        l3->showSteden();
        delay(speed);
    }
}
```