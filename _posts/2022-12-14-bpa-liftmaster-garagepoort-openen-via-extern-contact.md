---
title: "De BPA / Liftmaster garagepoort extern aansturen"
date: 2022-12-14
categories: 
  - "home-automation"
tags: 
  - "loxone"
coverImage: "image-5.jpeg"
---

Toen ze bij mij de BPA Lion [garagepoort zijn komen plaatsen](https://tomclaus.be/bouw/bouw-huis/de-garagepoort-en-voordeur-zijn-geplaatst/), was de documentatie zeer beknopt over het extern aansturen van de motor. Na enkele dagen research zijn we erachter gekomen hoe we deze toch kunnen aansturen via een extern contact, schakelaar of Loxone.

### Aansluitingen BPA / Liftmaster

De BPA en Liftmaster-modellen hebben de volgende aansluitingen op de motor.

![Afbeelding](/images/image-11-e1695204635438.png)

**1 + 2 — Pulse Poort Open/Sluit/Stop**

Wanneer je contact `1` + `2` verbind door middel van een korte puls zal de poort openen of sluiten. Indien deze in al beweging is zal de poort stoppen.

**0 + 2 — Pulse Lamp aan/uit**

Door contact `0` \+ `2` te verbinden, zal je de lamp van de Liftmaster aan en uit schakelen.

**2 + 3 — Lichtbarriere contact**

Is bedoeld voor een extern contact op aan te sluiten om te kijken of er een object aanwezig is tussen de poort. Hierop sluit je bijvoorbeeld een infraroodoog op aan. Indien de poort gaat sluiten, en je verbind contact `2` en `3`, zal deze stoppen en terug open.

**3 + 4 — Deurcontact**

Hierop sluit je een deurcontact aan (indien er een deur in je poort zit). Indien je deze contacten verbind, zal de poort niet geopend kunnen worden.

**6 + 7 — In beweging**

Hierop kan je een 24v zwaailamp aansluiten. Indien je dit wenst kan je hierop ook '_uitlezen_' of de poort in beweging is.

### Sturen via Loxone

Om je BPA / Liftmaster te sturen via Loxone, kan je gewoon een relaiscontact aansluiten (via UTP) op contact `1` en `2` van je BPA/Liftmaster. Via de Poort bouwsteen kan je dit relaiscontact aansluiten op de `Qtr` uitgang, om zo de poort te openen en/of sluiten.

### Status poort te weten komen

Sommige garagepoort kan je uitlezen om te weten te komen of je **poort open en toe** is. Mijn poort had deze functie niet dus wou ik met contacten werken om dit te weten te komen. Echter was dit moeilijk te monteren op mijn poort zelf en de muur. Daarom had ik besloten om **2 raamcontacten** op de rail zelf te monteren wat een perfect resultaat geeft.

![Liftmaster garagepoort gaat open](/images/image-8.jpeg)

![Liftmaster garagepoort contacten](/images/image-7.jpeg)

![Afbeelding](/images/image-6.jpeg)

![Liftmaster garagepoort contact open](/images/image-5.jpeg)

![Liftmaster garagepoort contact gesloten](/images/image-4.jpeg)

Deze contacten kan je dan eenvoudig inlezen in de Loxone Poort bouwsteen op de ingangen `Dc` en `Do`.

![Afbeelding](/images/image-12.png)
