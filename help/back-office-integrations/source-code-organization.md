---
title: Läs om Commerce Integration Starter Kit med nyckelmappar och automatiseringsskript
description: Läs om hur du ordnar källkoden i startpaketet för Commerce Integration. ​
landing-page-description: Utforska Source Code Organization i en Commerce Integration Starter Kit
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: 11daa4b29dafe5fcecf6b75cf2269b87f65c4612
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Source-kodorganisation för Adobe Starter Kit

Läs om källkodsorganisationen i startpaketet för Adobe Commerce Integration. &#x200B; Utforska projektets struktur och markera nyckelmappar som `actions` och `scripts` samt deras respektive innehåll. &#x200B; Mappen Actions innehåller undermappar som `ingestion` och `webhook` som innehåller viktig kod för händelsehantering och spårning. Du kommer också att lära dig mer om mapparna `starter-kit-info` och `scripts`. Mappen `scripts` fokuserar på automatiseringsskript som `commerce-event-subscribe` och `onboarding` som effektiviserar händelsekonfigurationen och providerinställningarna i projektet.
&#x200B;
Utforska logiken bakom källkodsstrukturen och beskriv hur mapparna `commerce` och `external` under varje entitetsmapp hanterar händelser som kommer från olika system. I videon förklaras vilken roll mappen `consumer` har när det gäller att skicka händelser till lämplig åtgärd i händelsehanteraren, vilket säkerställer smidig bearbetning. I videon visas även den återförsöksmekanism som implementerats i körningsåtgärderna för att hantera misslyckade händelser effektivt. &#x200B;Förstå källkodens struktur och funktionalitet i startpaketet för Adobe Commerce Integration, med värdefulla insikter om händelsehantering, automatiseringsskript och konfigurationsinställningar.

## Målgrupp

* Utvecklare som vill förstå hur källkoden är ordnad i nyckelmappar som `actions` och `scripts`.
* Läs mer om mappen `actions` innehåller undermappar som `ingestion` och ` webhook` som innehåller viktig kod för att hantera händelser och spåra distributioner.
* Utvecklare som vill veta mer om mappen `actions` som innehåller mappar för entiteter som `customer`, `order`, `product` och `stock`.

## Videoinnehåll

* Förstå att de fyra huvudmapparna: `actions`, `scripts`, `test` och `utils`, med fokus på mapparna `actions` och `scripts` under sessionen. &#x200B;
* Lär dig mer om mappen `actions` och hur den innehåller viktiga undermappar som `ingestion` och `webhook`.
* Utforska mappen `actions` och varför det finns specifika mappar för entiteter som `customer`, `order`, `product` och `stock`, som var och en innehåller körningsåtgärder som är strukturerade i mapparna `commerce` och `external` för att hantera händelser från Commerce och tredjepartssystem effektivt. &#x200B;
* Lär dig hur viktigt det är att inte ändra koden i mappen `starter-kit-info`, som innehåller en körningsåtgärd som används av Adobe för att spåra projektdistributioner baserat på startpaketet. &#x200B;
* Förstå mappen `scripts` som innehåller automatiseringsskript som `commerce-event-subscribe` och `onboarding` som automatiserar händelsekonfigurationen, providerkonfigurationen och konfigurationen av modulen Adobe I/O-händelser i Commerce. &#x200B;

  >[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

  {{$include /help/_includes/starter-kit-related-links.md}}