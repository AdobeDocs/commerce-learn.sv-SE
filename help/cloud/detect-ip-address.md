---
title: Identifiera IP-adresser
description: Lär dig att identifiera IP-adresser för Adobe Commerce Cloud-miljöer för att förbättra säkerheten och effektivisera serverkommunikationen
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: b015b9c64be631b43ad63d180c003dda8fdd198a
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# Identifiera IP-adresser för olika miljöer

Lär dig hur du identifierar IP-adresser för olika miljöer i ett Adobe Commerce Cloud-projekt. Genom att använda en rad kommandon, bland annat Adobe Commerce CLI, sed, xargs, dig, grep och sort-u, kan man identifiera IP-adresser för utvecklings-, staging- och produktionsmiljöer.

## Vem är den här videon till?

* Utvecklare: vill förstå hur du samlar in IP-adresser för Adobe Commerce Cloud-projektet.
* DevOps och säkerhetsteam som behöver begränsa åtkomsten till system från tredje part eller backend

## Videoinnehåll {#video-content}

* Lär dig hur du hittar IP-adressen för alla miljöer i Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## Kommando för att hämta IP-adressen

Observera att du måste använda ditt projekt-ID och miljönamnet i stället för platshållarinformationen.  Det kan också finnas ett behov av att ändra `{1..3}` så att det matchar antalet noder i Adobe Commerce Cloud-klustret, men 3 är det vanligaste.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

CLI-verktyget magento-cloud är utformat för att hjälpa utvecklare och systemadministratörer att hantera projekt och miljöer i Adobe Commerce Cloud på ett effektivt sätt. Det utökar funktionaliteten i molnkonsolen så att användarna kan utföra rutinuppgifter och köra automatisering lokalt. Bland huvudfunktionerna finns hantering av integreringsmiljöer, utcheckning och sammanslagning av miljöer, listning av variabler och användning av SSH för att ansluta till fjärrmiljöer. Verktyget förenklar arbetsflödena genom att kommandon kan köras direkt från den lokala arbetsstationen, vilket förbättrar den övergripande utvecklings- och driftsättningsprocessen.

I det här inledande avsnittet av exempelkoden begär `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1` URL:en för miljön. Det returnerade värdet ser ut ungefär så här `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`. Då och då ser den mer ut som `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Det här första kommandot är ganska enkelt, och nu är det dags att gå vidare till nästa kommando.

Mer information finns i [Cloud CLI-översikt](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Använder `sed` för sökning och ersättning

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

Kommandot `sed` i UNIX®Linux® står för Stream Editor. Den används för att utföra grundläggande textomformningar på en indataström (en fil eller indata från en pipeline). Vanliga användningsområden är att söka, söka och ersätta, infoga och ta bort text. Kommandot `sed` bearbetar textrad för rad och tillämpar angivna åtgärder, vilket gör det till ett kraftfullt verktyg för textredigering och skriptning.

Som tidigare nämnts finns det normalt två typer av URL:er som returneras från klippet `magento-cloud`. Det finns en variation som innehåller `.com.c.c` i mitten. Den här varianten måste ändras. Om den här strukturen identifieras måste allt tas bort från början av URL:en till och med `.com.c.c`.  Sedan är det som återstår bara den sista delen av URL:en. Ett exempel-URL ser ut som `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  När mönstret identifieras är målet att behålla allt efter `.c.`.  I det här exemplet används `sed 's/.\.c\.(.)/\1/'` för att hämta den här delen och ignorera resten av det ursprungliga returnerade värdet. Den återstående delen av URL:en liknar `abcikdxbg789.ent.magento.cloud/`.\
Två kommandon körs i `sed`. De avgränsas av ett semikolon. Den andra delen av mitt `sed`-kommando `;s/.$//'` är att ta bort eventuella efterföljande snedstreck för att rensa URL:en så att den ser ut som `abcikdxbg789.ent.magento.cloud`.  Nu har URL:en rensats och är klar för nästa kommando.

## Xargs with digdig

```bash
xargs -I% dig +short {1..3}."%"
```

Kommandot `xargs` i UNIX®Linux® används för att skapa och köra kommandorader från standardindata. Den hämtar indata från en pipe eller en fil och konverterar den till argument för ett annat kommando. Det är särskilt användbart för hantering av ett stort antal argument som överskrider skalets gräns. Kommandot `xargs` kan användas för att utföra åtgärder som att flytta, kopiera eller ta bort filer. Den möjliggör effektiv gruppbearbetning genom att skicka flera argument till kommandon i en enda körning.

Kommandot `dig`, som är en förkortning av Domain Information Groper, är ett nätverksadministrationsverktyg som används för att fråga efter DNS-servrar (Domain Name System). Det hjälper till att hämta information om DNS-poster, t.ex. A-, AAAA-, MX- och CNAME-poster. Kommandot `dig` används ofta för att felsöka DNS-problem, verifiera DNS-konfigurationer och samla in detaljerad information om domännamn och deras associerade IP-adresser. Genom att använda olika alternativ och flaggor kan användarna anpassa utdata för att visa specifik information eller en kortfattad sammanfattning.

Användningen av `xargs` med `dig` gör det komplicerat, men det är nödvändigt. Målet är att ta bort den rensade URL:en och spara den.  När URL:en har sparats som variabel infogas den i kommandot `dig`.

Kommandot `dig` skapades för att samla in DNS-information. För att minska mängden data som returneras används argumentet `+short`. Genom att använda `dig` i kombination med `+short` returneras IP-adresser och ibland strängar.

I den delen av kommandot sparar `xargs` den URL:en `abcikdxbg789.ent.magento.cloud` och infogar den i nästa kommando `dig`. Tekniken med att spara URL:en i kombination med iteration gör det enklare att använda med kommandot `dig`. Kom ihåg att min exempelkod är ett sätt att uppnå målet, och att du kan ändra saker som uppfyller dina behov och förväntningar.

Nu är URL:en klar. Nu ska vi se hur det går att kontrollera varje server i klustret. För Adobe Commerce Cloud har varje server i klustret ett nummer. Serveridentifieraren är ett prefix till URL:en som just har rensats och gjorts klar för användning. Ett snabbt och enkelt sätt att checka ut servrarna är att använda `{1..3}`. Genom att använda `{1..3}` som meddelar kommandot `dig` att köras 3 gånger. Nedan visas vad som händer om du tittar på körningen i realtid.

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

I illustrationssyfte påminner den här exempel-URL:en som används av `dig` om:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

Om `{1..3}` ändrades till `{1..6}` skulle det se ut så här:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## Kommandot `grep`

Ibland returneras en sträng som en del av resultatet från `dig`. I detta syfte är målet bara IP-adresser och de består av siffror med punkter. Om du vill vara säker på att det slutliga resultatet bara innehåller siffror kan du justera kommandot. När det är klart är den sista syntaxen ` grep '^\d'`.  Genom att lägga till `'^\d'` sparar kommandot `grep` bara siffror och ignorerar allt annat.

## Kommandot `sort`

Genom att använda `sort -u` tas alla dubbletter bort från listan över IP-adresser. Dubbletter inträffar bara när du tittar i utvecklingsnivåer.

Dessa lågnivåmiljöer är multi-tenant och delar underliggande servrar med många andra projekt. Utvecklingsmiljöer är enskilda servrar och aldrig ett kluster. När digeringskommandot upprepas för varje iteration returneras därför samma IP många gånger. Med kommandot `sort -u` tas alla dubblerade IP-adresser bort och bara unika IP-adresser återstår.



## Relaterad dokumentation

* [Regionala IP-adresser](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
