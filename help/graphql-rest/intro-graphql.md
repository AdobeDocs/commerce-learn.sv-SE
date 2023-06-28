---
title: GraphQL introduktion
description: Lär dig använda GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Använda GraphQL GETS- och POST-samtal för Adobe Commerce och [!DNL Magento Open Source].
landing-page-description: Lär dig använda GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Använda GraphQL GETS- och POST-samtal för Adobe Commerce och [!DNL Magento Open Source].
short-description: Lär dig använda GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Använda GraphQL GETS- och POST-samtal för Adobe Commerce och [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# GraphQL introduktion

GraphQL har snabbt blivit branschstandard för hur kraftfulla applikationer på klientsidan kommunicerar med en serverdel. Det är ett allt viktigare ämne för Adobe Commerce-utvecklare, eftersom plattformen fortsätter att utöka sina funktioner när det gäller headless-implementationer.

Om du inte har använt GraphQL tidigare orienterar det här avsnittet dig efter grundläggande begrepp och användningsområden.

## Vad är GraphQL?

GraphQL är en specifikation för ett unikt API-frågespråk och körningsmiljön som tillhandahåller data som svar på det frågespråket.

Traditionella webb-API:er som REST har fungerat bra för olika system som skickar data fram och tillbaka, men har gett mindre än topprestanda för moderna applänksupplevelser som Progressive Web Application. I program som detta är de främre och bakre skikten i _samma_ program kommunicerar via webb-API. Den registrerade metoden för scheman som REST ger ofta inte lämplig flexibilitet i detta sammanhang, där många typer av data måste hämtas snabbt.

GraphQL tillåter en kund att beskriva uttryckligt _exakt_ de data som behövs. I stället för att kräva flera nätverksbegäranden för hämtning av flera datatyper kan en enda begäran fråga efter många typer. Och svaren förblir smala genom att endast de typer och fält som efterfrågas inkluderas (i ett format som intuitivt speglar frågan).

Körningsmiljön som implementerar GraphQL-specifikationen kan byggas på vilket språk som helst. Adobe Commerce och [!DNL Magento Open Source] använder
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP-implementering och skapar egna lager ovanpå den.

[Se den fullständiga dokumentationen för GraphQL](https://graphql.org/learn){target="_blank"}

## Använda en GraphQL-klient

Du behöver en GUI GraphQL-klient för att testa kodexempel och självstudiekurser. Det finns flera alternativ:

* [Altair](https://altairgraphql.dev/){target="_blank"} är en utmärkt och fullt utrustad klient som byggts specifikt för GraphQL. Adobe använder Altair i genomgångsvideor.
* Om du inte vill installera skrivbordsprogrammet finns det även Altair-tillägg som kan köras i
  [Krom](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} webbläsare.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} är en implementering av GraphQL IDE från GraphQL Foundation. Detta är inte ett installationsbart verktyg, utan ett paket som du kan använda för att bygga gränssnittet själv.
* Om du redan är bekant med [Postman](https://www.postman.com/){target="_blank"}, har programmet bra stöd för GraphQL-frågor, även om det inte är lika bra som en dedikerad GraphQL-klient.

I din GraphQL-klient bör du skicka dina förfrågningar till URL-sökvägen `/graphql` på din Adobe Commerce eller [!DNL Magento Open Source] -instans. Om du föredrar att använda en befintlig instans för dina tester kan du använda demonstrationen av Venia-temat (exempelimplementeringen av PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
