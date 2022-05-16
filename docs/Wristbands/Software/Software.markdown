---
layout: default
title: Software
parent: Wristbands
grand_parent: Puzzels
nav_order: 2
---


# Software

[Visit our Github to find our code!](https://github.com/PLAN-IT-B/BachelorProefWristbands/tree/main/Wristband-Code)

## Algemeen

De code bestaat uit 3 grote delen. Als eerste hebben we onze variabelen, daarnaast hebben we al onze functie en dan ook nog de setup en de loop. We zullen in deze paragraaf proberen uit te leggen hoe onze code is opgebouwd.

## Libaries en includes

In onze code gebruiken we 3 libaries. We gebruiken enerzijds eentje om io-pinnen van onze esp aan te sturen. Daarnaast gebruiken we ook een libary voor het manipuleren van de de hartslagsensor, en een laatste voor de MQTT connectie.

```ini
lib_deps = 
	erropix/ESP32 AnalogWrite@^0.2
	sparkfun/SparkFun MAX3010x Pulse and Proximity Sensor Library@^1.1.1
	knolleary/PubSubClient@^2.8
```

```c
#include "Arduino.h"
#include <string.h>
#include <Wire.h>
#include "MAX30105.h"
#include "analogWrite.h"
#include "heartRate.h"
#include <BLEDevice.h>
#include <BLECast.h>
#include "PubSubClient.h" 
#include "WiFi.h"
```

## Variabelen

### Unieke variabelen

Het eerste deel van de code bevat 2 belangerijke variabelen.
namelijk de naam van de esp en zijn ID. Die laatse wordt tijdens het verloop van het spel veranderd omdat deze wordt gebruikt voor de partnerruil. 

Elke wristband heeft zijn eigen espNaam en id. Het vreemd is dat ze als volgt worden gegeven:

    esp1 : ID = 3
    esp2 : ID = 4
    esp3 : ID = 1
    esp4 : ID = 2

Dit wordt zo gedaan zodat de eerste code die wordt verstuurd naar de vuilbakken niet "1234" is, maar "3412".  Dit wordt zo gedaan zodat het spel niet te makkelijk start.
    
```c
    int id = 3;
    std::string espNaam = "esp1";
```

OPGELET: Wanneer aan de esp's andere id's worden gegeven moet de variabele "doorstuurCode" ook aangepast worden.

### Aanpasbare variabelen

Vervolgens hebben we een deel variabelen die we kunnen aanpassen naargelang we bepaalde parameters in het spel willen veranderen.

```c
    int codetime =5000;
    int codetimelong =60000;
    const unsigned long switchPeriod = 600000;
    int partnerTime = 30;
    int grenswaardeTeVer  = 50;   
    int grenswaardeTeDicht = 50; 
    int irThreshold = 70000;
    int hartslagFoutenMax = 5;
    const unsigned long period = 1000;
    int foutenMarge =5;
```
* **codetime**: 
Tijdens het spel wordt een code duidelijk gemaakt door knipperlichtjes. Als de code 3412 is, dan gaat esp3 eerst een ledje laten knipperen, vervolgens knipperen respectievelijk esp4, esp1, esp2 ook een ledje. De tijd die hiervoor tussen het knipperen van twee opeenvolgende esp's is hier voorgesteld in milliseconden.

* **codetimelong**: De code die wordt beschreven in vorig puntje wordt in dit geval elke minuut opnieuw getoont.

* **switchPeriod**: Tijdens de duur van de escaperoom wordt er ook gedaan aan partnerruil. Deze variabele heeft aan na hoeveel milliseconden dit gebeurd. In dit geval duurt dit 10 minuten.

* **patnerTime**: In het begin van een nieuwe ronde van 10 minuten krijgen de spelers een aantal seconden tijd om hun partner te vinden. Tijdens deze periode worden geen fouten gestuurd naar de buffer.

* **grenswaardeTeVer** en **grenswaardeTeDicht**: Dit is een grenswaarde om te beslissen wanneer twee esp te dicht of te ver bij elkaar zijn. Deze waarde stelt de sterkte van het ontvangen BLE-signaal voor.

* **hartslagFoutenMax**: Wanneer er geen hartslag word gedetecteerd wordt er niet direct een foutmelding gestuurd naar de buffer. De fout moet zich namelijk 5 keer na elkaar voordoen vooralleer deze wordt gestuurd. Dit doen we om te voorkomen dat er bij een foute meting van de hartslagsensor direct een grote fout zou worden gestuurd.

* **period**: elke esp stuurt continu een signaal. Na een bepaalde periode van, in dit geval, één seconde zal de esp zijn omgeving scannen.

* **foutenMarge**: Er wordt niet telkens wanneer iemand te ver of te dicht is een kleine fout gestuurd. Dit wordt pas gedaan wanneer er in het spel telkens 5 fouten zijn gemaakt.

### Vaste variabelen 
Verder hebben we heel wat variabelen die nodig zijn voor het spel te kunnen programmeren, maar ook voor het besturen van de componenten en de communicatie.

__Random variabelen__

Dit onderdeel bevat verschillende die we gebruiken in de code.
```c
    //VARIABLES RANDOM ============================================================
    const char *cEspNaam = espNaam.c_str();
    BLECast bleCast(espNaam);                  
    int codePeriod = 10000000;
    std::string doorstuurCode ="Wristband-code 3412";
   
    int foutCounter = 0;
    int ronde = 0;

    int codetimelongcounter =0;
    
    
```
Zo zorgen de eerste twee variabelen voor de naam van de esp die we later gebruiken om een unieke naam te hebben bij de MQTT-communicatie.

De **codePeriod** wordt telkens aangepast in de loop. Dit wordt gelijk gesteld aan **codetime + id*codetime;**. Dit gebruiken we om de delay voor het knipperen van de leds te realiseren.

De **doorstuurCode** is de code die wordt doorgestuurd naar de vuilnisbakken. Deze wordt aangepast wanneer er aan partnerruil wordt gedaan en een nieuwe code wordt gecreerd.

Verder hebben cwe nog een variabele die het aantal fouten bijhoud, en een variabele die bijhoudt in welke ronde van het spel we zitten. Met rondes bedoelen we periodes waarin de partners dezelfde blijven.

Als laatste hebben we de variabele **codetimelongcounter**. Deze heeft misschien een iets wat verwarrende naam, maar deze is bedoelt om bij te houden hoeveel keer dat de code tijdens een ronde is weergegeven op de esp's. 

**variabelen en constanten voor de WIFI**

Volgende variabelen zijn uiterst voor de WIFI en de MQTT communicatie. Zo definiëren we een paswoord en wifi-id, maar ook het ip-adres en de poort. Verder hebben we variabelen die ons helpen met het manipuleren van messages.
```c
    //VARIABLES AND CONSTANTS WIFI ============================================================
#define SSID          "NETGEAR68"
#define PWD           "excitedtuba713"

#define MQTT_SERVER   "192.168.1.2"
#define MQTT_PORT     1883

WiFiClient espClient;
PubSubClient client(espClient);
long lastMsg = 0;
char msg[50];
int value = 0;
```

```c
**variabelen en constanten voor de hartslagsensor**
//VARIABLES  AND CONSTANTS HARTSLAGSENSOR ==============================================
MAX30105 particleSensor;

const byte RATE_SIZE = 4; //Increase this for more averaging. 4 is good.
byte rates[RATE_SIZE]; //Array of heart rates
byte rateSpot = 0;
long lastBeat = 0; //Time at which the last beat occurred

float beatsPerMinute;
int beatAvg;

bool hartratesensorEnable;
int hartslagFoutenTeller;

```

```c
//Constants RGB LED ============================================================
const int PIN_RED   = 16;
const int PIN_BLUE = 4;
const int PIN_GREEN  = 2;
```

```c
//VARIABLES AND CONSTANTS BLE ============================================================
BLEScan *pBLEScan;

    int d1;  //afstand tot ESP1
    int d2;  //afstand tot ESP2
    int d3;  //afstand tot ESP3
    int d4;  //afstand tot ESP4

    int distance1;  //afstand tot id1
    int distance2;  //afstand tot id2
    int distance3;  //afstand tot id3
    int distance4;  //afstand tot id4



    const int scanTimeSeconds = 1;
    uint8_t cnt = 0;
    char data[5];
    uint8_t mode;


```

```c
//VARIABLES TIMING ============================================================

    unsigned long startMillis;  
    unsigned long currentMillis;

    unsigned long switchStartMillis;
    unsigned long switchCurrentMillis;

    unsigned long codeStartMillis;
    unsigned long codeCurrentMillis;
```













