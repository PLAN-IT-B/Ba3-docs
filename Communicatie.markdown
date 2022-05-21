---
title: Communicatie
nav_order: 3
---

# Communicatie

---
## Communicatie methode
---

![](MQTT_broker.svg)

### Publish-subscribe messaging pattern
In de software-architectuur is publish-subscribe een  messaging pattern waarbij de verzenders van berichten, publishers genaamd, de berichten niet programmeren om ze rechtstreeks aan specifieke ontvangers, subscribers genaamd, te zenden, maar in plaats daarvan de gepubliceerde berichten in directories in te delen zonder te weten welke subscribers er eventueel zijn. Op dezelfde wijze geven de subscribers hun belangstelling voor een of meer directory te kennen en ontvangen zij alleen de berichten die hun belangstelling hebben, zonder te weten welke publishers er zijn, als die er al zijn.

Dit is ideaal voor een escaproom en heeft veel vrijheid gegeven aan de verschillende ontwerp teams.

---
## Netwerktopologie
---

--- FOTO TOEVOEGEN VAN NETWERK TOPOLOGIE ---

---
## Keuze protocol
---

De éné keer moet er iets gesignaleerd worden, de andere keer moeten meerdere puzzels tergelijk op de hoogte worden gesteld over de status van een bepaalde puzzels.

We hebben besloten dat elke puzzel zijn eigen directory heeft op de MQTT broker. Hierop kunnen er aftakking zijn, die specifiekere informatie kan bieden. Het formaat van de message wordt onderling tussen de teams afgesproken, en aangezien we een kleine groep zijn, was dit ideaal.

**Bijvoorbeeld:**
* We hebben de directory *"TrappenMaar"*, deze heeft directory "*Trappenmaar/buffer"* en "*Trappenmaar/zone"*. Op de directory van "*Trappenmaar/buffer"* zal de buffer esp filteren op messeges die hier toekomen. Hierop kan een message *"grote fout"* of *"kleine fout"* gelezen worden.  

    Maar er is ook een directory zoals "*Trappenmaar/zone"*, bij een verandering van de zone van de buffer wordt er hier gepublished welke zone het nu is. Nu ligt de verantwoordelijkheid bij de andere puzzels om te beslissen wat ze willen doen met deze informatie.

**Voor meer duidelijkeid over wat wie wanneer stuurt, zal er op deze site bij elke puzzel een pagina over cummunicatie zijn.**

---
## Opmerking:
--- 
Na de eerste week had de communicatie verantwoordelijke al samengezeten met alle teams over welke berichten ze willen ontvangen van wie, en naar wie ze wat willen sturen. 

Dit heeft deze persoon dan in een overzichtelijk word document gestoken, genaamd *"[Datacommunicatie.docx](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/blob/main/Documentatie%20communicatie/Datacommunicatie.docx)"*, dit gaf alle puzzels veel structuur, en zo konden ze al coderen, zonder te wachten op de andere puzzels. 

Dit samen met de zijn *"[Basic-communicatie-code](https://github.com/PLAN-IT-B/BachelorProefCommunicatieEnEinde/tree/main/Documentatie%20communicatie/Basic-communicatie-code)"* zorgde ervoor dat alle teams snel wegwaren met deze manier van communiceren.