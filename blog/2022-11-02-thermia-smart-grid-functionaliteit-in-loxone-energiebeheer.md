---
title: "Thermia Smart Grid functionaliteit in Loxone Energiebeheer"
date: 2022-11-02
categories: 
  - "home-automation"
tags: 
  - "loxone"
  - "thermia"
coverImage: "t1.1556788449.6853-e1700217105211.jpg"
---

Indien je beschikt over een recente Thermia (Calibra) warmtepomp, dan kan je Smart Grid functionaliteit hiervan eenvoudig integreren in Loxone. Op die manier heb je de volgende mogelijkheden:

- Zware verbruikers van de warmtepomp uitschakelen wanneer er een piek verbruik is. Bijvoorbeeld bij het inschakelen van de inductiekookplaat.

- Je warmtepomp extra werk laten verrichten bij overtollige energie van je zonnepanelen. Hierbij zal je boiler wat warmer gestookt worden, alsook je vloerverwarming zal enkele graden warmer gezet worden.

## Aansluitingen

De warmtepomp beschikt 2 contacten die je kan sluiten via een Loxone relais. Hiervoor moet je wel een extra (UTP) kabel leggen tussen je warmtepomp en je Loxone installatie.

Deze contacten bevinden zich links op de BM-card van je warmtepomp en hebben de nummer 409 & 408.

![Afbeelding](/images/image-1.png)

**BOOST**  
**Warmte** — Gewenste kamertemperatuur is ingesteld op boostwaarde (standaard 22°C).

**Buffertank** — Stelt de gewenste tanktemperatuur in op 55°, en indien ‘Allow immersion heater’ tijdens boost’ is ingeschakeld, wordt ook het interne verwarmingselement gebruikt.

**Kraanwater** — Stelt de kraanwatermodus in op Boost. Start- en stoptemperatuur is ingesteld om waarden te verhogen (standaard starttemperatuur 55°C en stoptemperatuur 65°C).

_Start- en stoptemperatuur niet instelbaar voor Altas en Calibra-warmtepomp._

### Werking

Als beide contacten open staan heeft je warmtepomp een normale werking. Echter wanneer je één of beide contacten sluit ga je het gedrag aanpassen waarbij je de volgende 4 functies kan krijgen.

| VU/SG-1 (409) | SG-2 (408) | Functie |
| --- | --- | --- |
| 0 | 0 | **NORMAAL**   Normale werking |
| 0 | 1 | **COMFORT**   \- **Verwarming** — Gewenste kamertemperatuur is ingesteld op comfortwaarde (standaard 22°C).   \- **Kraanwater** — Stelt de modus in op Comfort. De Start- en stoptemperatuur is ingesteld op de Comfort-waarden (standaard starttemperatuur 45°C en stoptemperatuur 60°C).   _Start- en stoptemperatuur niet instelbaar voor Altas en Calibra-warmtepomp._ |
| 1 | 0 | **EVU**   De functie blokkeert de werking van de compressor en het interne verwarmingselement. Andere componenten zoals circulatiepompen, directionele kleppen en externe bijverwarming worden nog steeds geregeld.   Communicatie-alarm van de omvormer en alarmen van de omvormer zijn uitgeschakeld tijdens EVU. |
| 1 | 1 | **BOOST**   **\-** **Warmte** — Gewenste kamertemperatuur is ingesteld op boostwaarde (standaard 22°C).   \- **Buffertank** — Stelt de gewenste tanktemperatuur in op 55°, en indien ‘Allow immersion heater’ tijdens boost’ is ingeschakeld, wordt ook het interne verwarmingselement gebruikt.   \- **Kraanwater** — Stelt de kraanwatermodus in op Boost. Start- en stoptemperatuur is ingesteld om waarden te verhogen (standaard starttemperatuur 55°C en stoptemperatuur 65°C).   _Start- en stoptemperatuur niet instelbaar voor Altas en Calibra-warmtepomp._ |

## Loxone Config — Energiebeheer

### Energiemanager (Overtollige PV energie gebruiken)

Via de [Energiemanager](https://www.loxone.com/nlnl/kb/energiemanager/) zullen we overtollige PV energie, die we normaal op het net zouden zetten, nuttig inzetten in ons huis. Bijvoorbeeld om de boiler van onze warmtepomp wat extra werkt te laten doen.

Op deze manier kunnen we ons verbruik na zonsondergang beperken doordat de boiler volledige ‘opgeladen is’ en dus voord e avond lekker warm water kan afgeven zonder energie te moeten verbruiken.

#### Lastmanager (Houd rekening met je capaciteitstarief)

Via de [Lastmanager](https://www.loxone.com/nlnl/kb/lastmanager/) zullen we ervoor zorgen dat de warmtepomp bepaalde functionaliteiten zal uitschakelen wanneer er andere zware verbruikers reeds actief zijn. Bijvoorbeeld wanneer we aan het koken zijn.

Op deze manier kunnen we ons totaal vermogen dat we afnemen van onze energieleverancier beperken, alsook ons capaciteitstarief beperken.

## Loxone Config

![Afbeelding](/images/image.png)

In het bovenstaande voorbeeld zie je dat bij overtollige Energie naar net (-8.0kW), de uitgang Q1 van de Energiemanager ingeschakeld zal worden. Op die manier activeren we de 2 relais op onze installatie en sluiten we dus de 2 contacten van de warmtepomp. Op deze manier zal de Thermia warmtepomp in Boost modus gaan.

De ingestelde parameters zijn momenteel nog een een beetje zoeken. Maar momenteel heeft de [energiemanager](https://www.loxone.com/nlnl/kb/energiemanager/) de volgende paramaters voor Q1:

- **Activeringsduur**: 0s

- **Min. inschakelduur**: 5min

- **Min. Uitschakelduur**: 10min

- **Min. Inschakelduur/dag**: 0min

- **Einde Min. Inschakdelduur/dag**: Zonsondergang

- **Inschakelvermogen**: 3kW

- **Vermogen**: 2kW _(Dit kan best aangepast worden nagelang het verbruik van je warmtepomp)_

Voor de [Lastmanager](https://www.loxone.com/nlnl/kb/lastmanager/) is er maar 1 parameter en dat is die van je maximale vermogen dat je wil afnemen van je energieleverancier. Momentele staat dit op 5kW ingesteld voor de klant.

[](https://medium.com/@tomclaus?source=post_page-----1bde527654ec--------------------------------)
