---
title: Optimera återanvändning av kod med Adobe Commerce
description: Lär dig hur du optimerar återanvändning av kod i Adobe Commerce med mönster för global referensarkitektur, vilket förbättrar prestanda och efterlevnad i flera instanser.
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Medverkad av Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Medverkad av Tony Evers"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# Implementeringstekniker för global referensarkitektur

Det finns flera sätt att optimera återanvändning av kod med Adobe Commerce. Dessa fyra implementeringstekniker har sina egna fördelar. Exemplen i den här artikeln ordnas från enkla till mer komplexa. Välj den strategi som bäst passar ditt projekt och framtida färdplan. En migrering från en strategi till en annan kan vara tidskrävande.

## När den globala referensarkitekturen ska användas

Den globala referensarkitekturen kan vara värdefull, beroende på hur många instanser du äger. En instans är en fristående installation av Adobe Commerce som använder sin egen databas. Räkna antalet produktionsdatabaser för att ta reda på hur många instanser du äger. Om du underhåller mer än en instans, eller om du förutser detta scenario i framtiden, kan du dra nytta av den globala referensarkitekturen. Ju fler funktioner instanserna delar desto mer värde lägger en global referensarkitektur till.

I alla dessa scenarier är det tillrådligt att använda flera instanser av Adobe Commerce.

1. **Olika butiksägare**: Om du underhåller kod för flera butiksägare, där var och en har en egen separat butik, kan det behövas separata instanser för att effektivt underhålla de enskilda kraven.
2. **Efterlevnad av nationella bestämmelser**: Vissa bestämmelser föreskriver att kunddata måste lagras inom specifika regioner. I sådana fall är det nödvändigt med olika instanser för att säkerställa att dessa bestämmelser följs.
3. **Operativa avvikelser mellan geografiska regioner**: Om du arbetar i flera regioner kan det innebära olika underhållsplaner och krav. Om du använder separata instanser kan du hantera dessa variationer på ett flexibelt sätt.
4. **Försäljning av Flash med hög intensitet**: Butiker som säljer blixt i stor skala kräver ofta optimerade serverprestanda. Dedikerad infrastruktur som tillhandahålls av olika instanser säkerställer optimala prestanda under sådana efterfrågade perioder.
5. **Betydande skillnader mellan varumärken eller länder**: När skillnaden mellan varumärken eller länder är stor, resulterar en enda instans i kod som bara används för vissa varumärken eller länder. Separata instanser kan förbättra prestanda och stabilitet genom att eliminera onödig kod för varumärken och länder som inte behöver den.

## Global Reference Architecture Patterns

Bredvid&quot;inget GRA-mönster&quot; finns det fyra typer av GRA-mönster.

![5 ikoner för GRA-mönster: inga GRA, split, bulk, separate och monorepo.](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### Inget GRA-mönster

![En ikon som visar &quot;ingen GRA&quot;](/help/assets/global-reference-architecture/no-gra.png){align="center"}

När ett GRA-mönster inte används är varje Adobe Commerce-instans ett unikt program. Det finns ingen återanvändning av kod förutom genom att manuellt flytta kod från en instans till en annan. De här kopiorna skiljer sig alltid åt. Mängden arbete som krävs för att säkerställa att alla instanser har samma ändringar men fortfarande fungerar som väntat kan bli överväldigande. I det här scenariot behöver tre instanser mer än en instans underhållsinsats.

![Ett diagram som visar 3 butiker, där varje är en kopia från den första, med unik utveckling i alla 3 kopior.](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### Det delade Git GRA-mönstret

![En ikon som visar GRA-mönstret &quot;split&quot; &#x200B;](/help/assets/global-reference-architecture/split-git.png){align="center"}

Det här mönstret består av Git-databaser för utveckling och en Git-databas per instans. Varje fil i instansen lagras i en av utvecklingsdatabaserna. De förenas som en flest som utgör hela GRA. Varje kodrad finns bara i en enda utvecklingsdatabas och installeras till instanserna med hjälp av strecktekniken, vilket leder till att koden återanvänds.

![Ett diagram som visar var kod lagras i ett delat GRA-mönster](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### GRA-mönster för grupppaket

![En ikon som representerar GRA-mönstret&quot;bulk&quot;](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Adobe Commerce kärnmoduler och tredjepartsmoduler installeras direkt via Composer-databaser. Git-databaser kan användas som Composer-databaser. I det här mönstret lagras hela den delade GRA-kodbasen i en eller flera Git-databaser och installeras via Composer. Den viktigaste egenskapen är att det finns flera moduler, språkpaket eller teman i ett enda dispositionspaket, vilket förenklar utvecklingen.

![Ett diagram som visar var kod lagras i ett GRA-mönster för grupppaket](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### GRA-mönstret för de separata paketen

![En ikon som representerar GRA-mönstret&quot;Separata paket&quot; &#x200B;](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Varje Adobe Commerce-modul, språkpaket eller tema installeras som ett separat dispositionspaket. Varje anpassning har en egen Git-databas. Det innebär total flexibilitet i instansens komposition och har tillförlitlig Composer-beroendehantering. För prestandaoptimering speglas alla paket i en enda privat dispositionsdatabas.

![Ett diagram som visar var kod lagras i ett separat paket, GRA-mönster](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### GRA-mönstret för monorepo

![En ikon som representerar GRA-mönstret&quot;monorepo&quot;](/help/assets/global-reference-architecture/monorepo.png){align="center"}

All utveckling sker i en enda koddatabas. Automatisering genererar paket för nya versioner och publicerar dem i en databas för disposition. I mönstret kombineras de låga utvecklingskostnaderna för satsmetoden med flexibiliteten i metoden med separata paket. Monorepomönstret är också idealiskt för att köra automatiska funktionstester.

![Ett diagram som visar var kod lagras i ett GRA-mönster för monorepo &#x200B;](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Välja ett GRA-mönster

Valet för GRA-mönster görs genom att man bedömer projektets komplexitet, behovet av flexibilitet och utvecklingsteamets förmåga att anpassa sig.

Team med lite erfarenhet av Adobe Commerce får bäst resultat. Om projektet kräver ett mer komplext GRA-mönster på grund av dess egenskaper ska det dock inte kompromissa.

Vanliga projektegenskaper för varje mönster:

1. **Inget GRA-mönster**: En instans av Adobe Commerce utan planer på att utöka. Flera instanser av Adobe Commerce med minimal samverkan mellan dem.

2. **Dela Git GRA-mönster**: Team som inte vill använda Composer för sina anpassningar, är i de flesta fall Mönster för grupppaket att föredra framför Dela Git.

3. **GRA-mönster för grupppaket**: Anpassningskodebas med högt beroende. Alla instanser har mycket lika kombinationer av anpassade paket. Sälja och degradera enskilda paket. Team med liten erfarenhet av kodhantering och behöver vara enkla.

4. **Separat paket, GRA-mönster**: Flexibel versionsomfångshantering krävs. 50 anpassade paket eller mindre förväntas inom de kommande 5 åren. Globala och regionala lager av gemensam kod kan finnas. Inga planer på att migrera till ett monorepo-mönster. Teamet är tekniskt skickligt och har strikt processledning.

5. **Monorepo GRA-mönster**: Alla egenskaper för det separata pakets GRA-mönster. Mer än 50 förpackningar förväntas under de kommande 5 åren. Behovet av omfattande automatiserad testning. Stöd för tillfälliga miljöer. Komplex kodbas i företagsklass som behöver skalas om utan att underhållskostnaden behöver skalas upp till samma pris.

### Kostnaden för fel val

Det går att migrera från ett mönster till ett annat. Bilden nedan visar hur stor påverkan det har om man går från ett mönster till ett annat. Gröna linjer har låg påverkan, gula och gula linjer är måttligt komplexa till komplexa migreringar.

![Ett diagram med färgade pilar mellan alla 4 GRA-mönster, vilket anger svårighetsgraden av att flytta mellan olika mönster.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
