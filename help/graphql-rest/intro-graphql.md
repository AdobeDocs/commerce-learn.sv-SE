---
title: GraphQL introduktion
description: Lär dig hur du använder GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Använd GraphQL GET- och POST-samtal för Adobe Commerce och [!DNL Magento Open Source].
short-description: Lär dig hur du använder GraphQL samtal för GET och POST för Adobe Commerce och  [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# GraphQL introduktion

Detta är en del av serien för GraphQL och Adobe Commerce. GraphQL har snabbt blivit branschstandard för hur kraftfulla applikationer på klientsidan kommunicerar med en serverdel. Det är ett allt viktigare ämne för Adobe Commerce-utvecklare, eftersom plattformen fortsätter att utöka sina funktioner när det gäller headless-implementationer.

Om du inte har använt GraphQL tidigare orienterar det här avsnittet dig efter grundläggande begrepp och användningsområden.

>[!VIDEO](https://video.tv.adobe.com/v/3443944?learn=on&captions=swe)

## Relaterade videor och självstudiekurser om GraphQL i den här serien

* [Del 2 GraphQL - Frågor](../graphql-rest/graphql-queries.md)
* [Del 3 GraphQL - Mutationer](../graphql-rest/graphql-mutations.md)
* [Del 4 GraphQL - schema](../graphql-rest/graphql-schema.md)

## Vad är GraphQL?

GraphQL är en specifikation för ett unikt API-frågespråk och körningsmiljön som tillhandahåller data som svar på det frågespråket.

Traditionella webb-API:er som REST har fungerat bra för olika system som skickar data fram och tillbaka, men har gett mindre än topprestanda för moderna applänksupplevelser som Progressive Web Application. I program som detta kommunicerar de främre och bakre skikten i _samma_ -programmet via webb-API:t. Den registrerade metoden för scheman som REST ger ofta inte lämplig flexibilitet i detta sammanhang, där många typer av data måste hämtas snabbt.

GraphQL tillåter en klient att uttryckligen beskriva _exakt_ de data som krävs. I stället för att kräva flera nätverksbegäranden för hämtning av flera datatyper kan en enda begäran fråga efter många typer. Och svaren förblir smala genom att endast de typer och fält som efterfrågas tas med (i ett format som intuitivt återspeglar frågan).

Körningsmiljön som implementerar GraphQL-specifikationen kan byggas på vilket språk som helst. Adobe Commerce och [!DNL Magento Open Source] använder
[ graphql-php ](https://webonyx.github.io/graphql-php/){target="_blank"}  PHP-implementering och skapar egna lager ovanpå den.

[Visa den fullständiga GraphQL-dokumentationen](https://graphql.org/learn){target="_blank"}

## Använda en GraphQL-klient

Du behöver en GUI GraphQL-klient för att testa kodexempel och självstudier. Det finns flera alternativ:

* [Altair](https://altairgraphql.dev/){target="_blank"} är en utmärkt och fullt utrustad klient som skapats speciellt för GraphQL. Adobe använder Altair i genomgångsvideor.
* Om du inte vill installera skrivbordsprogrammet finns det även Altair-tillägg som kan köras i
  [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox eller [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} är en implementering av GraphQL IDE från GraphQL Foundation. Detta är inte ett installationsbart verktyg, utan ett paket som du kan använda för att bygga gränssnittet själv.
* Om du redan är bekant med [Postman](https://www.postman.com/){target="_blank"} har den stöd för GraphQL-frågor, även om den inte är lika komplett som en dedikerad GraphQL-klient.

I din GraphQL-klient bör du skicka dina förfrågningar till URL-sökvägen `/graphql` på din Adobe Commerce- eller [!DNL Magento Open Source]-instans. Om du föredrar att använda en befintlig instans för dina tester kan du använda demonstrationen av Venia-temat (exempelimplementeringen av PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
