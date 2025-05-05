---
title: Läs om alternativ för katalogimport som medföljer Adobe Commerce
description: Läs om hur du kan importera kataloger till din Adobe Commerce-butik med några av de inbyggda alternativen.
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
source-git-commit: b0fe49352b00a68554e662327cd66983c30d8285
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# Alternativ för att importera en katalog

Det finns några inbyggda metoder för att importera en katalog till Adobe Commerce. Varje metod har en egen motivering för hur den används tillsammans med pros och cons som måste beaktas.

Välj något av alternativen nedan om du vill veta mer.

>[!BEGINTABS]

>[!TAB Manuell]

## Skapa produkterna manuellt {#manual-import}

Om du har en begränsad katalog och det inte finns så många uppdateringar kan det vara bäst att skapa dem manuellt. Det tar tid att gå in i respektive produkt och lite tid att lära sig hur man använder Commerce Admin. Manuell kataloghantering är inte rätt alternativ för de flesta butiker, men i vissa situationer kan det vara bra. Mer information om processen finns på [Skapa en produkt](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html?lang=sv-SE){target="_blank"}. Glöm inte att du kan använda mer än en metod för att hantera katalogen, men när du väl har använt automatisering måste manuella redigeringar vara begränsade. Automatiska uppdateringar kan skriva över ändringar som har utförts manuellt och därför skapa förvirring. När integreringen med Adobe Commerce för att hantera katalogen använder automatisering och API:er bör du begränsa kataloghanteringen från administratören till [användarroller och behörigheter](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html?lang=sv-SE){target="_blank"}.

### När den här metoden ska övervägas

- Mycket liten katalog, till exempel färre än 50 produkter
- Uppdateringar är ovanliga
- Du har all produktinformation, bilder, videor och du vill inte ta dig tid att lära dig hur du konverterar data till CSV
- Du vill lägga till bilder och videoklipp när du skapar produkterna
- Ditt team är `not` bekant med API:er och hur OAUTH fungerar

>[!TAB Admin CSV]

## Administratörens CSV-importverktyg {#admin-csv}

Med det här verktyget kan en butiksägare importera en katalog via en CSV-fil direkt från e-handelsadministratören.
[Importera data från Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=sv-SE){target="_blank"}

Yrkesverksamma:
Att överföra en CSV-fil från administratören är ett enkelt sätt att hantera kataloger. Det ger snabbare katalogproduktuppdateringar till en katalog i måttlig storlek.

Kon:

- Långsam
- Den maximala överföringsfilens storlek har definierats på servern och kan eventuellt inte enkelt justeras av en butiksägare.
- Kräver administratörsåtkomst och någon att utföra åtgärden, automatiseringen är begränsad
- Tidsplanerade importer är begränsade till högst 1x per dag
- De bilder och videoklipp som är associerade måste överföras separat

### När den här metoden ska övervägas

- Katalogstorleken är måttlig
- Uppdateringar får inte vara mer än en gång om dagen
- du har viss åtkomst till serverkonfigurationer om du måste öka den maximala filöverföringsstorleken
- Ditt team är `not` bekant med API:er och hur OAUTH fungerar

>[!TAB MassREST API]

## Bulk REST API {#bulk-rest-api}

Med REST API:t för större datamängder kan man automatisera och få uppdateringar oftare. Det här API:t är snabbare än att överföra CSV-filer till administratörer.
[Dokumentation för massslutpunkter](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Yrkesverksamma:
Möjlighet att importera stora datauppsättningar som inte är i CSV-format.

Kon:

- De bilder och videoklipp som är associerade måste överföras separat
- Kan begränsas av bandbreddsbegränsningar för värdtjänstleverantören

### När den här metoden ska övervägas

- Katalogen har alla storlekar
- Det är vanligt att det finns uppdateringar, mer än 1x om dagen kan användas
- Det är viktigt att importera tiden, men den är inte avgörande och en kort fördröjning av bearbetningen av importdata kan accepteras
- Data är inte strukturerade i CSV-format och du kan inte omvandla dem med automatisering

>[!TAB ASYNC REST API]

## ASYNC REST API {#async-rest-api}

En asynkron webbslutpunkt fångar upp meddelanden till ett webb-API och skriver dem till meddelandekön. Varje gång systemet accepterar en sådan API-begäran genereras en UUID-identifierare. Adobe Commerce inkluderar detta UUID när meddelandet läggs till i kön. Sedan läser en konsument meddelandena från kön och kör dem en i taget.
[Asynkron dokumentation för webbslutpunkter](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Yrkesverksamma:

- Snabb import av data
- Butiksomfånget stöds eller så kan du ange att `all` ska utföra åtgärden i alla befintliga butiker

Kon:

- GET-begäran stöds inte

### När den här metoden ska övervägas

- Importen är vanlig
- Inga problem med en liten fördröjning från det att de skickas via API och sedan bearbetas från meddelandekön.


>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

Det här API-alternativet tillåter extremt snabba importer jämfört med alla andra inbyggda alternativ.

[Importera data REST CSV API](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Yrkesverksamma:

- Den snabbaste metoden att bearbeta inkommande data
- Kan utföras flera gånger per dag
- Data kan komprimeras med gzip för stora begäranden för att undvika storleksbegränsningar för HTTP-begäranden.

Kon:

- De bilder och videoklipp som är associerade måste överföras separat
- Data måste vara i ett CSV-format

### När den här metoden ska övervägas

- Katalogen har alla storlekar
- Det är vanligt att det finns uppdateringar, mer än 1x om dagen kan användas
- Den övergripande tiden för import är viktig
- Data är redan i CSV-format eller kan enkelt omvandlas med automatisering

>[!ENDTABS]

## Ytterligare resurser

- [Importera data med ny REST CSV](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [Importera huvuddokumentation för data](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=sv-SE){target="_blank"}
- [Versionsinformation för Adobe Commerce version 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html?lang=sv-SE){target="_blank"}
- [Användare, roller och behörigheter](../site-management/users-roles-permissions.md){target="_blank"}
