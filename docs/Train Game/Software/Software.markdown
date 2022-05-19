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

### loop
//Comunicatie
   if (!client.connected())
  {reconnect();}

  client.loop();
  long now = millis();

  if (now - lastMsg > 5000)
  {
    lastMsg = now;
  }
// Ticket
  if(deel1){
    t2->volgendeStad(zone);
    if(tt1_once){
      t2->setStad(4);
      tt1_once = false;
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,ticketWidth,ticketHeight,ticket);
    }
    tt1->TicketCheckTrace(t2->getStad());
    Serial.print(tt1->getTicketCheckcomplete());
    Serial.println();
    if(!tt1->ticketGetAllesKits()){
      t2->setStad(tt1->ticketGetBegin());
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,wrongWidth,wrongHeight,wrong);
      l1->reset();  //rood
      tft.fillScreen(TFT_WHITE);
      tft.pushImage(61,21,ticketWidth,ticketHeight,ticket);
      tt1->ticketSetAllesKits(true);
      client.publish("TrappenMaar/buffer","kleine fout");
    }
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
      client.publish("treingame/4decijfer",CodeChar); //code sturen naar garbage
      Serial.println("Trein-code " + String(codeDeel1,DEC));
      deel1=false;
      deel2=true;
    } 
  }

## Joystick

## LEDS

## LCD

## Trein

## TrainTrace