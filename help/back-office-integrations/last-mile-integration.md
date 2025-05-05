---
title: Integrering på sista mils plats i startpaketet för Commerce-integrering.
description: Senaste mils integrering i Commerce, som framhäver utökningsmöjligheter som validering, omvandling, förbearbetning, sändning och efterbearbetning. ​
landing-page-description: Lär dig strukturen och funktionerna i utbyggbara kopplingar på sista mils integrering för Commerce-system.
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Integrering på sista mils avstånd med Adobe Starter Kit

Lär dig mer om saker du bör tänka på när du startar integreringen med Adobe Commerce på sista milen med fokus på att använda utbyggbara kopplingar för att förbättra anslutningen till tredjepartssystem. I den här videon beskrivs en strukturerad metod där olika kopplingar, t.ex. validering, omvandling, förbehandling, sändning och efterbearbetning, ger ett smidigt dataflöde och systemsynkronisering. Varje krok har ett tydligt syfte, som

* Validerar inkommande data mot scheman
* Omforma dataobjekt mellan system
* Utföra beräkningar innan relevant information skickas
* Skicka data till målsystemet

Det är viktigt att ha separata JavaScript-filer för varje block för att upprätthålla affärslogiks integritet och underlätta framtida uppgraderingar av ramverket, vilket ger en robust och anpassningsbar konfiguration.

Lär dig mer om betydelsen av efterbehandlingsaktiviteter via postproceskroken, som gör det möjligt för användare att utföra ytterligare åtgärder efter datasynkronisering, som att lägga till kommentarer i order eller lagra externa ID:n. Videon innehåller metodtips som att kapsla in API-begäranden i specifika bibliotek för att effektivisera anslutningar med tredjepartssystem. Du får också lära dig typiska användningsexempel för varje krok och vägledning om hur du hanterar olika scenarier.

## Målgrupp

* Utvecklare som vill lära sig strukturen och funktionaliteten hos utökningsbara kopplingar och hur dessa kan förbättra anslutningen till tredjepartssystem.
* Utvecklare som vill lära sig typiska användningsfall och bästa praxis för varje utökningsbar krok, till exempel validering, omvandling, förbehandling, sändning och efterbearbetning, för att underlätta smidigt dataflöde, systemsynkronisering och effektivt underhåll av integreringskonfiguration. &#x200B;

## Videoinnehåll

* Lär dig mer om strukturen för de anropade åtgärderna i integreringen på sista milen.
* Förstå typiska användningsfall inom valideringkroken, inklusive validering av inkommande data mot scheman och att hoppa över specifika händelser baserat på vissa kriterier. &#x200B;
* Lär dig vilken roll omvandlingskroken har när det gäller att omforma dataobjekt mellan ursprungs- och målsystemen.
* Lär dig mer om betydelsen av den skicka kroken när det gäller att underlätta den faktiska datameddelandet som skickas till målsystemet.

>[!VIDEO](https://video.tv.adobe.com/v/3451924?learn=on&captions=swe)

{{$include /help/_includes/starter-kit-related-links.md}}
