---
title: "Gira Dual Q rookmelders"
date: 2022-11-05
categories: 
  - "home-automation"
tags: 
  - "gira"
  - "loxone"
coverImage: "images.jpeg"
---

Ik krijg regelmatig de vraag van welke rookmelders je best gebruikt in een Smart Home. Als Loxone verdeler zou het logisch zijn dat ik ook de [Loxone rookmelders](https://shop.loxone.com/nlnl/rookmelder-air.html) zou aanraden, maar toch geniet de [Gira Dual Q](https://partner.gira.com/en/produkte/sicherheit/rauchmelder/rauchmelder-dual.html) momenteel onze voorkeur.

### Waarom de Gira Dual Q rookmelder

Het grote verschil tussen de Gira Dual Q en de Loxone rookmelder zit hem in de manier van communiceren en uitlezen. Het is zo dat wanneer je een Loxone rookmelder hebt en deze detecteert brand, dat je hiervan een push bericht krijgt in Loxone. Echter blijven de andere rookmelder stil bij brand detectie.

Bij grote woningen kan dit een risico’s met zich meebrengen. Bijvoorbeeld wanneer er brand is in de inpandige garage, zal dit nauwelijks te horen zijn op de 2de verdieping van de woning.

Gelukkig beschikt de Gira Dual Q rookmelder over verschillende contacten waarmee je deze rookmelders kan verbinden met elkaar. Hierdoor zullen deze allemaal in alarm gaan, wanneer er ééntje rook detecteert.

Andere voordelen van de Gira Dual Q is dat je via een contact ook het alarm op commando kan laten afgaan. Bijvoorbeeld bij een inbraakalarm. Alsook dat je deze kan opladen via 12v.

### Loxone Rookmelder

**Voordelen**

- Eenvoudig te implementeren

- Draadloos — eenvoudig overal te plaatsen

**Nadelen**

- Air extension noodzakelijk

- Niet verbonden met elkaar

- Geen alarm op commando

### Gira Dual Q

**Voordelen**

- Verbonden rookmelders

- Oplaadbare batterij via 12V

- Alarm op commando (bv. bij inbraakalarm)

**Nadelen**

- Bekabeling noodzakelijk

- Digitale inputs op Loxone nodig

- Rookmelders verbinden

Om de rookmelders met elkaar te verbinden moet je deze allemaal parallel verbinden zoals het schema in de handleiding:

![](/images/image-3.png)

Wanneer 1 rookmelder brand detecteert zal deze het contact tussen de 2 draden verbinden en zo het alarm van de andere rookmelders in werking zetten.

### Manueel alarm activeren

Echter kunnen we deze 2 verbinden ook op een relais-contact aansluiten van Loxone. Op die manier kunnen we ook vanuit Loxone de rookmelders in alarm laten gaan. Bijvoorbeeld bij en inbraakalarm.

### Alarm uitlezen in Loxone

Indien je 1 rookmelders van het Gira Relais Contact voorziet kan je deze aansluiten op een Digitale Ingang van Loxone. Op die manier kan je een melding krijgen op je telefoon wanneer de rookmelders brand detecterenm

### Rookmelder opladen

Een ongedocumenteerde feature is dat je op het 3de klemmetje van 12v spanning kan zetten. Op die manier is de rookmelder 24/7 van spanning voorzien. Dit is wel aangewezen indien je de rookmelder ook voor inbraakalarm gaat gebruiken.

![](/images/image-2.png)
