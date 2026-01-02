---
title: "Thermia warmtepomp uitlezen en sturen via Modbus en Loxone"
date: 2022-11-04
categories: 
  - "home-automation"
tags: 
  - "loxone"
  - "thermia"
coverImage: "IMG_4454.jpg"
---

Indien je beschikt over een recente Thermia Mega, Atlas, Calibra of Diplomat warmtepomp, dan kan je deze uitlezen en aansturen via Loxone door middel van het Modbus prototol.

Deze modellen van Thermia beschikken namelijk over de [Thermia Online (Genesis)](https://thermia.com/improved-thermia-online-genesis/) webinterface. Alsook hebben deze modellen twee Modbus-protocollen ter beschikking:

- Modbus RTU (aansluiten op de BM-kaart)

- Modbus TCP / IP (aansluiten via RJ45-poort op de unit)

In de warmtepomp instellingen en handleidingen zal je regelmatig de afkorting BMS terugvinden, dit staat voor Building Management System. Wat wil zeggen dat ze integreerbaar zijn met smart homes.

## Aansluiten via Modbus RTU

> Om van start te gaan moet je uiteraard ook beschikken over de [Loxone Modbus Extension](https://shop.loxone.com/nlnl/modbus-extension.html), anders zal het niet mogelijk zijn deze aan te sluiten via Modbus RTU.

1. Schakel je Loxone installatie en Thermia warmtepomp volledig uit.
2. Sluit de Modbus kabel eerst aan op je Loxone Modbus Extension.
3. Open het front-paneel van je warmtepomp
4. Je Thermia warmtepompen heeft een groene 3-delige connector met de tekst BMS/MBe.

![Afbeelding](/images/image-6.png)

1. Sluit de kabel aan op de MBe connector

- Loxone Modbus A connector = Thermia D+ connector

- Loxone Modbus B connector = Thermia D- connector

1. Schakel je Loxone installatie en warmtepomp terug in.

In de _“display settings”_ van je Thermia moet je nu nog de _BMS (Building Management System)_ optie activeren. Controleer of je de volgende instellingen hebt:

- Modbus mode: RTU

- Address: 1

- Baudrate: 19200

- Parity : Even

- Stop bits: 1

In Loxone kan je nu een Modbus apparaat toevoegen onder je Modbus Extension. Je Thermia Warmtepomp heeft standaard Modbus-Adres 1, waaronde rje de verschillende registers kan toevoegen.

## Aansluiten via Modbus TCP/IP

Het aansluitem van je Thermia warmtepomp via Modbus TCP gebeurd via de gewone Ethernet kabel.

In de _“display settings”_ van je warmtepomp moet je de _BMS_ optie activeren. Controleer of je de volgende instellingen hebt:

- Modbus mode: TCP/IP

- Port: 502

Volg nu de volgende stappen in je Loxone Config:

1. Maak je bij “Miniserver Communicatie” een nieuwe modbusserver aan.
2. Geef deze de benaming “Thermia Modbus Server”.
3. Vul het IP-Adres van je Thermia warmtepomp in bij “Adres” samen met poort 502
4. Maak een Modbus Apparaat aan met de naam van je Thermia Warmtepomp
5. Nu kan je de verschillende Modbus registers toevoegen in Loxone.

![Afbeelding](/images/image-4.png)

## Thermia’s Modbus Register

Na contact te hebben met support van Thermia Sweden heb ik van hun de volgende handleiding gekregen. Deze bevat alle informatie om de warmtepomp uit te lezen of aan te sturen.

[Modbus-protocol-for-Genesis-platform-10](https://www.tomclaus.be/wp-content/uploads/2023/09/Modbus-protocol-for-Genesis-platform-10.pdf)[Download](https://www.tomclaus.be/wp-content/uploads/2023/09/Modbus-protocol-for-Genesis-platform-10.pdf)

**_In deze handleiding zal je tabellen terugvinden met de verschillende modbus registers. In deze tabellen zal je enkel een kolom Mega en Diplomat terugvinden. De kolom van Diplomat is ook geldig voor Atlas en Calibra warmtepompen._**

![Afbeelding](/images/image-5.png)

[](https://medium.com/@tomclaus?source=post_page-----b733433700a3--------------------------------)
