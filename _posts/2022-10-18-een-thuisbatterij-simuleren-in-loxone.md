---
title: "Een thuisbatterij simuleren in Loxone"
date: 2022-10-18
categories: 
  - "home-automation"
tags: 
  - "loxone"
coverImage: "cabinet_p1-e1700217278108.jpg"
---

Wie al een tijdje met het idee speelt om een thuisbatterij te plaatsen, blijft zichzelf rijk _(of arm)_ rekenen. Daarom bouw ik tegenwoordig voor klanten een batterij-simulator. Deze simulator houd rekening met reële cijfers op basis van je verbruik en PV-opbrengst. Daarnaast houd deze simulator ook rekening met de limieten van een thuisbatterij.

## Omvormer

Onze simulatie bouwen we op aan de hand van verschillende componenten. Als eerst gaan we aan de slag om de batterij omvormer (Inverter) te simuleren.

Als input `AI1` gebruiken we met het vermogen dat we normaal het net opsturen of afnemen. Deze parameter halen we op uit onze energiemeter die net voor onze (digitale) teller geplaatst is.

Als input `AI2` gebruiken we de huidige capaciteit van onze batterij. _(Deze input bouwen we later…)_ Deze input hebben we nodig om te weten of onze batterij volledig vol is, er nog capaciteit over is of dat deze volledig leeg is. Zo zullen we bijvoorbeeld als onze batterij vol is, geen extra vermogen in onze batterij steken en gaat dit deze energie het net op, de `AQ` uitgang zal dan op 0 staan. In de andere gevallen zal `AQ` het vermogen weergeven dat we in- of uit onze batterij moeten halen.

![Afbeelding](/images/image-2.jpeg)

Verder moet je weten dat elke omvormer een maximaal oplaad en ontlaad vermogen heeft. We kunnen dus bijvoorbeeld niet zomaar de 6kW die beschikbaar en normaal het net opduwen, volledig in onze batterij steken. In ons geval heeft de omvormer een maximaal vermogen van 4.000W, dit voor zowel het opladen alsook ontladen van de batterij. Deze berekening zit ook mee in onze status-bouwsteen.

![Afbeelding](/images/image-3.jpeg)

## Batterij

Om ons vermogen in kW om te zetten naar een teller in kWh, maken we gebruik van een Verbruik Teller Bouwsteen. Deze teller werkt perfect met het systeem van opladen en ontladen wat een ideale teller voor deze situatie is.

Op ingang `P` zetten we het vermogen dat we in de batterij kunnen steken. Op de uitgang `Aq` zien we dan het beschikbare vermogen van onze batterij. In ons voorbeeld heeft een volle batterij een capaciteit van 10kWh.

![Afbeelding](/images/image-2.jpeg)

## Het resterende vermogen voor/van Leverancier

Nu we energie in onze batterij steken, wil dit zeggen dat we ook minder op het net zetten. We moeten dus een nieuwe parameter hebben die weet wat onze nieuw rest-vermogen berekend dat het net opgaat of die we afnemen van onze leverancier.

Hiervoor doen we en simpele berekening, tussen het echte cijfers minus het vermogen dat we opnemen in onze batterij simulatie.

![Afbeelding](/images/image.jpeg)

_In dit voorbeeld zie je dat we normaal 5kW op het net zouden zetten, maar dat we nu 4kW in onze batterij steken. Onze rest-vermogen dat het net opgaat is dus 1kW._

## De Energie Monitor

Nu we alle nodige parameters hebben, kunnen we onze Energie Monitor Bouwsteen gaan opbouwen.

We gebruiken de volgende ingangen:

- `Pp` Het vermogen dat onze zonnepanelen momenteel leveren

- `Pv` Het vermogen dat we het net opsturen (-) of afnemen (+)

- `Ps` Het vermogen dat in onze batterij steken (+) of mee ontladen (-)

- `Ss` Het percentage van de beschikbare capaciteit van de batterij

![Afbeelding](/images/image-2.jpeg)

## Resultaat

### Alles samen in Loxone Config

![Afbeelding](/images/image-10.png)

### Het resultaat in onze Loxone App

![Afbeelding](/images/image-7.png)

![Afbeelding](/images/image-9.png)

![Afbeelding](/images/image-8.png)

### Extra 1 — Besparing teller

Verder heb ik nog een aparte teller gekoppeld aan de Energie Monitor, om mijn effectieve bespraring te tellen. Hiermee bedoel ik de elektriciteit die ik normaal zou moeten aankopen bij Fluvius, maar nu uit mijn batterij kan ontladen. Zo kan ik eenvoudig op maandelijkse basis zien hoeveel kWh ik voordeel doe met een batterij.

Hiervoor gebruik ik de min-max _(0 tot 100)_ begrenzer om zo enkel het positieve vermogen te tellen van mijn batterij omvormer. Zo weet ik het vermogen dat ik ontlaad uit mijn batterij en kan ik dit in een teller opslaan.

![Afbeelding](/images/image-1.jpeg)

![Afbeelding](/images/image-8.png)

Nu kan je in één oogopslag je bespaarde kWh zien en vermenigvuldigen met de elektriciteitsprijs waarin je hem normaal moest aankopen. In mijn geval zou dat 20 cent zijn. _(24 cent aankoopprijs minus 4 cent wat ik niet meer op het net zou zetten)_
