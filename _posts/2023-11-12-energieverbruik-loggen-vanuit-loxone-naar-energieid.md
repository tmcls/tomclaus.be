---
title: "Energieverbruik loggen vanuit Loxone naar EnergieId"
date: 2023-11-12
categories: 
  - "home-automation"
tags: 
  - "energieid"
  - "energyid"
  - "loxone"
  - "monitoring"
coverImage: "verbruik-opvolgen-1024x556-1.webp"
---

![Afbeelding](/images/verbruik-opvolgen-1024x556.webp)

Zelf gebruik ik [EnergieId](http://energyid.eu) voor het bijhouden en analyseren van mijn energieverbruik. Zo kan ik eenvoudig op lange termijn mijn verbruik opvolgen.

Er zijn bij EnergieId tal van automactische imports mogelijk, zo kan je je verbruik van Fluvius automatisch inladen, een koppeling leggen met je Fronius zonnepanelen of zelf een custom webhook opzetten om data te pushen naar hun.

Via zo een webhook gaan we in dit geval data pushen naar hun. In mijn voorbeeld is dit voor het verbruik van mijn Warmtepomp door te sturen naar EnergieId. In Loxone krijg ik deze verbruiksgegevens binnen via een modbus energiemeter binnen Loxone. Indien je vebruiksgegevens via een andere manier binnen krijgt in Loxone is het ook perfect mogelijk om door te sturen naar EnergieId.

## EnergieId Webhook

1. Via de [Webhook App](https://app.energyid.eu/integrations/WebhookIn) binnen EnergieId kan je een nieuwe webhook aanmaken.

3. Koppel deze correct aan je **dossier** en geef deze een **omschrijving** zoals "_Loxone_".

5. Vervolgens krijg je een **Webhook URL**. Deze heb je later nodig om in Loxone in te geven.  
    Zo een webhook opgebouwd als volgt `https://hooks.energyid.eu/services/WebhookIn/XXXX-XXXX-XXXX/XXXXX`

## Loxone Setup

### **Virtuele Uitgang**

Maak een **Virtuele Uitgang** aan in je Loxone Config met de volgende parameters

- **Benaming**: `EnergieID`

- **Adres**: `https://hooks.energyid.eu`  
    _Let op: geen / op het einde_!

Vervolgens voegen we onder deze Virtuele Uitgang een **Virtuele Uitgang Commando** toe met de volgende parameters:

- **Benaming**: `Warmtepomp`

- **Commando bij AAN**: `/services/WebhookIn/XXXX-XXXX-XXXX/XXXXX`  
    _Dit is je persoonlijke webhook, laat het voorste stuk "https://hooks.energyid.eu/" weg._  
    _Let op: zet zeker een / vooraan!_

- **HTTP header bij AAN**: `Content-Type: application/json`

- **HTTP Body bij AAN**: `<v>`  
    Indien je exact wil weten wat al deze parameter zijn, [verwijs ik graag door naar hun documentatie](https://api.energyid.eu/docs.html#webhook).

- **HTTP methode bij AAN**: `POST`

- **Als digitale uitgang gebruiken**: UITVINKEN

Dit zou er als volgt moeten uitzien:

![Afbeelding](/images/Screenshot-2023-11-17-at-11.09.23-1024x722.png)

### **Status bouwsteen**

Vervolgens gaan we een status bouwsteen instellen. Bij de **status tekst** vol je het volgende in:

`{"remoteId": "warmtepomp-id","remoteName": "Warmtepomp","metric": "finalElectricityConsumption","metricKind": "cumulative","unit": "kWh","interval": "PT1H", "data": [[ "<v.t>+01:00", <v1.2> ]] }`

De overige velden mag je allemaal laten zoals het is.

![Afbeelding](/images/Screenshot-2023-11-17-at-11.08.49-1024x572.png)

### Analoog geheugen

Om niet elke waardewijziging, maar enkel 1x per uur een waarde naar EnergieId te sturen, werken we met een Analoog geheugen bouwsteen.

Aan de ingang `V` van deze status bouwstaan koppelen we onze energiemeter van de warmtepomp. De enigste voorwaarde is dat je Energiemeter een waarde teruggeeft in kWh. Indien je enkel vermogensdata (in W of kW) ter beschikking hebt, kan je er nog altijd een Meter-blok tussen plaatsen. Deze kan een energieverbruik (in kWh) metern aan de hand van energievermogen (in Wh).

Aan de ingang `Set`, koppelen we de **Puls uur variabele**. Dese kan je vinden bij Tijd functies in je Config.

Vervolgens hangen we de uitgang `V` van ons **Analoog geheugen** aan de ingang `I1` van onze **status bouwsteen**.  
Aan de `Txt` uitgang van onze status bouwsteen hangen we dan weer onze webhook die we aangemaakt hebben.

![Afbeelding](/images/Screenshot-2023-11-17-at-11.07.03-1024x190.png)

Nu moet je enkel nog alles **opslaan op je Miniserver**. Van zodra deze herstart is en het klokslag XXu00 is, zal de webhook automatisch deze gegevens doorsturen naar EnergieId, Deze webhook zal ook automatisch de meter aanmaken bij hun indien deze nog niet bestaatdit moet je dit niet zelf doen.

![Afbeelding](/images/Screenshot-2023-11-15-at-14.43.55-1024x83.png)
