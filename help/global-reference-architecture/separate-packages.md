---
title: Global referensarkitektur för separata paket
description: Optimera Adobe Commerce med separat paket (GRA). Lär dig konfigurera, fördelar och bästa praxis för flexibel versionshantering av paket.
jira: KT-16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Medverkad av Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Medverkad av Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: cbddc4a3-602f-4208-85cd-b906d2b81f8b
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '2101'
ht-degree: 0%

---

# Det globala referensarkitekturmönstret för separata paket

{{only-for-on-prem-commerce-cloud}}

I den här guiden beskrivs hur du konfigurerar Adobe Commerce med GRA-mönstret (Separate Packages Global Reference Architecture).

GRA-mönstret för separata paket innehåller en Git-databas för varje vanligt paket och en Git-databas för varje Adobe Commerce-instans. Vanliga paket visas via Composer med en privat dispositionsdatabas.

Det här globala referensarkitekturmönstret är helt Composer-baserat och har utformats för att få ut maximalt av alla Composer-funktioner.

![Ett diagram som visar var kod lagras i ett GRA-mönster för separata paket](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## Fördelar och nackdelar med mönstret

Fördelar:

- Kod återanvänds via delade koddatabaser
- Fullständig flexibilitet vid paketinstallation - varje GRA-paket kan uppgraderas, nedgraderas eller backporteras individuellt
- Fullt stöd för semantisk versionshantering
- Inga särskilda verktyg, komplex infrastruktur eller särskilda förgrenade strategier krävs
- Stöd för alla pakettyper som Composer stöder

Nackdelar:

- Utvecklingen inom detta GRA-mönster är lite svårare i början, men det finns en liten inlärningskurva
- Möjlighet att distribuera kombinationer av paket som inte har utvecklats i samma konfiguration, kräver strikta testprocedurer

## Konfigurera Adobe Commerce med GRA-mönstret för separata paket

### Katalogstrukturen

Den slutliga katalogstrukturen för en fullständig Adobe Commerce-installation med GRA-mönstret för separata paket ser ut så här:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

Katalogerna `app/code`, `app/i18n` och `app/design` har utelämnats med syfte. Med GRA för separata paket installeras varje enskilt paket från Composer. Även om paketet bara installeras i en enda Adobe Commerce-instans.

### Förbered arkivdatabasen

Skapa en databas för den första Adobe Commerce-instansen, som representerar en webbutik för Brand X.

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installera Adobe Commerce med `bin/magento setup:install`. Genomför resultatet `app/etc/config.php`.

### Skapa paketdatabaser

Varje paket i det här globala referensarkitekturmönstret har sin egen Git-databas. Nedan visas exempel på paket som innehåller Adobe Commerce-moduler som representerar en GRA-modul, en tredjepartsmodul och en lokal modul.

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

Använd exemplen för att skapa egna paket.

### Skapa metapaketdatabaser

Metapackage styr omfattningen av den gemensamma GRA-kodbasen i detta GRA-mönster. De definierar vad som finns i grunden: vilken kombination av paketversioner som alltid installeras tillsammans. Ett exempel:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

Ovanstående kodutdrag är Composer.json för ett metapaket. Eftersom metapaket bara innehåller en Composer.json-fil och ingen annan kod. Koden ovan är också det fullständiga metapaketet. Lägg det i en Git-databas och du har en installerbar databas för metapackage Composer. Det kräver ett exempel på en GRA-modul och en tredjepartsmodul, liksom Adobe Commerce Core. Det kräver också en grå-komponentsgrund, som förklaras i nästa kapitel.

Metapaket är ett sätt att paketera utan att skapa beroenden mellan paketen. Även om det inte finns något tekniskt beroende mellan paketen kan du med ett metapaket få dem att installeras tillsammans. Om du behöver det här metapaketet i ditt projekt installeras alla paket eller metapaket som metapaketet kräver. Om du skapar ett tomt dispositionsprojekt och bara behöver det här paketet, installerar Composer Adobe Commerce och GRA och tredjepartsmodulen.

På så sätt kan du säkerställa att alla butiker innehåller samma uppsättning grundläggande paket.

Du kan definiera ett metapaket som definierar store x på liknande sätt. För detta krävs grundmetapaketet, som kräver hela GRA-stiftelsen, plus en lokal modul:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Metapaketet Brand-X är valfritt. Du kan också hoppa över varumärkesmetapaketet och kräva dessa beroenden direkt i ditt butiksprojekt för Composer. Fördelen med att skapa ett metapaket för lokala moduler är att du inte har några funktionsgrenar och förfrågningar om att hämta funktioner i Git-lagringsplatsen för butiken, bara i paketdatabaserna. Det är en säkerhetsåtgärd. Dessutom kan du välja att använda semantisk versionshantering på paketdatabaserna och använda olika Git-taggar i huvudprojektet, till exempel för att spåra namngivna releaser. Det är upp till dig.

### GRA Foundation-filer utanför leverantörskatalogen

Ibland måste du lagra filer utanför leverantörskatalogen. Till exempel `.gitignore`, filer som finns i katalogen `dev/` eller domänverifieringsfilerna. Pakettypen magento2-component är utformad för detta ändamål. Titta på <https://github.com/AntonEvers/gra-component-foundation>.

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

Paketet har typen magento2-component och innehåller en src-katalog som är värd för filer som kopieras till Adobe Commerce rotkatalog. Mappningen i den här filen kopierar `/src/gitignore` till `/.gitignore` i Composer-huvudprojektet.

På så sätt kan du göra filer utanför leverantörskatalogen till en del av GRA-basen.

### Utveckla en GRA-modul

Utveckling sker i leverantörskatalogen. Be Composer att installera dina grundpaket från källan. Då checkas paket ut från Git i stället för att installeras från ett hämtat arkiv.

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

Med det här kommandot har paket i antonevers-namnutrymmet checkats ut med Git. När du anger katalogen vendor/antonevers/module-gra anger du även Git-databasen module-gra. Nu kan du skapa, checka ut och sammanfoga grenar på plats och utveckla på det här sättet, direkt från leverantörskatalogen.

### Inkludera tredjepartsmoduler i GRA-stiftelsen

Lägg till tredjepartspaket i GRA-metapaketet. Om det inte finns någon tredjepartskod att installera från en Composer-databas skapar du ett paket för den. Skapa en Git-repo, lägg till paketinnehållet (allt som finns i app/code/Vendor/Package) och se till att det finns en giltig Composer.json-fil i arkivets rot. Nu kan du installera det här paketet via Composer.

## Konfigurera en privat Composer-databas

En privat databas är inte obligatorisk i den globala referensarkitekturen. Det gör driftsättningar och installation snabbare, minskar databaskonfigurationen i Composer.json och ökar säkerheten. Autentiseringsuppgifter till andra Composer-databaser och Adobe Commerce Marketplace lagras i din privata databas. Inga fler känsliga autentiseringsuppgifter medföljer koden eller på utvecklarens datorer.

Dessutom erbjuder vissa privata databaser extra funktioner som e-postmeddelanden när en av dina butiker innehåller en säkerhetslucka i ett av sina beroenden.

Långsproblemet är vad som inträffar när du har flera VCS-databaser i Composer.json. Varje Composer-databas måste läsas vid uppgradering och om 50 databaser för 50 paket har minst 50 gånger så stor belastning som en enda Composer-databas.

![Ett diagram som visar var långsamhet inträffar när en dispositionsdatabas saknas](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Ta med en Composer-spegling i form av en privat Composer-databas. Spegeln innehåller en kopia av alla paket från andra Composer-databaser samt alla Git-värdbaserade paket. Med en privat Composer-databas får du dessutom en mer detaljerad åtkomstkontroll.

Med Git-synkronisering kan en privat Composer-databas automatiskt identifiera nya paket i dina Git-databaser samt nya versioner av befintliga paket.

Du kan ha din egen privata databas hos Satis: <https://composer.github.io/satis/>. Se ett exempel på en offentlig databas på <https://antonevers.github.io/gra-composer-repository/>. Den här rapporten används som dispositionsdatabas i kodexemplen. Ytterligare åtgärder krävs för att göra en databas i Satis privat.

Det finns lösningar som du kan konfigurera och glömma bort: Privat Packagist <https://packagist.com/>, som har skapats av samma personer som har skrivit Composer eller JFrog Artifactory <https://jfrog.com/artifactory/>.

## Leveranskod

Med metapaket finns det tre steg för att leverera kod.

1. Sammanfoga ändringar i paket och version av ändrade paket.
2. (Valfritt, endast om nya paket har lagts till) Kräv de nya paketen i metapaket och versionera metapaketen.
3. (Valfritt, endast om nya paket läggs till) Kräv de nya metapaketen i Adobe Commerce och distribuera.

Distributionsomfånget styrs med paketversioner. Att skapa en stabil version av ett paket innebär att det här paketet är klart för produktionsdistribution.

Om du vill skapa en ny release kör du en uppdatering av dispositionen i Composer-huvudprojektet, som innehåller den fullständiga butiksinstallationen. Alla senaste versioner av paket är installerade.

## Versioner

Versionshantering i separata paket (GRA) är en synonym till taggningsmoduler i Git. Git-taggar skapar numrerade versioner av dina paket som Composer installerar.
Med rätt versionsmetod kan paketen flöda automatiskt, samtidigt som de förblir säkra.

Två exempel:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

I det här exemplet visas en strikt definition av beroenden. 3 paket krävs i exakta versioner. Uppdatering av disposition med det här metapaketet i installationen gör ingenting. De här tre paketen installeras alltid i dessa exakta versioner, även om det finns en nyare version.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

I det här exemplet visas en lös definition av beroenden. Med `~1.0` kan alla versioner av dessa paket installeras om de är större än eller lika med `1.0.0` och lägre än `2.0.0`, med en inställning för den högsta tillgängliga versionen. Du kan lära dig mer om hur du definierar versionsberoenden på <https://getcomposer.org/doc/articles/versions.md>:

> Operatorn ~ förklaras bäst av exemplet: `~1.2` motsvarar `>=1.2 <2.0.0`, medan `~1.2.3` motsvarar `>=1.2.3 <1.3.0`.

När du släpper en ny version av något av de paket som nämns installeras den automatiskt med Composer-uppdatering.

Använd semantisk versionshantering. Du kan lära dig allt om semantisk versionshantering på <https://semver.org/>. Vanliga frågor och svar är ett måste. Med semantisk versionshantering kallas siffrorna i &quot;1.0.0&quot; MAJOR.MINOR.PATCH. Mindre releaser av ett paket och korrigeringsfiler ska vara säkra att införa utan att programmet bryts.
Du kan automatiskt inkludera korrigeringsfiler och manuellt välja mindre uppgraderingar. Observera att det kostar extra kostnader genom att manuellt välja varje mindre ändring:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

Det här fungerar förstås bara om du använder semantisk versionshantering konsekvent, alltid. Och inte bara i metapaket, utan även kraven för dina reguljära paket bör definiera beroenden löst. Om du har ett strikt beroende i systemet begränsas det paketet till den strikta definitionen.

Du kan hitta dessa strikta beroenden genom att skriva Composer som är beroende av \&lt;paketnamn\>. Mer information finns i <https://getcomposer.org/doc/03-cli.md#depends-why>.

## Förgreningsstrategi

Du kan använda olika förgreningsstrategier för att stödja det globala referensstrategimönstret, förutsatt att huvudgrenen är den enda grenen där du versionshanterar dina paket. Om du har en version som spänner över flera grenar medför det en risk för att funktioner i olika versioner går förlorade slumpmässigt. Skapa bara stabila versioner på huvudgrenen.

Skapa endast funktionsgrenar i dina paketdatabaser. Finns inte i dina butiksinstallationsarkiv. Du kan fortfarande göra ändringar i din butik bara med Composer. Undvik behovet av Git-sammanslagningar i distributionsdatabasen.

Grentyper som är vanliga i förgreningsstrategier och databaser som de ska finnas i:

**Funktionsgrenar**: finns inte i dina paketdatabaser någon annanstans.

**Versionsgrenar**: skapas i alla databaser: paket, metapaket, lagra installationsdatabaser. När du planerar en release, grupperas ändringar i frisläppningsgrenar för paket innan versionshanteringen sker. Anta att du förbereder en release med kodnamnet &quot;Unicorn&quot;. Du kan skapa en gren för återgivning/återgivning i paket med ändringar. Sammanfoga vad som helst där och sedan kräva &quot;dev-release-unicorn as 1.4.0&quot; i ditt metapaket. Läs mer om alias för att se vad som händer där: <https://getcomposer.org/doc/articles/aliases.md>.

**QA/Dev-grenar**: liknar frisläppa grenar.

**Huvudgren**: måste finnas i alla databaser och ska alltid vara den gren som representerar produktion eller ett produktionsklart läge. Huvudgrenen är den där du taggar kod för att frisläppa versioner.
Se till att du väljer en förgreningsstrategi med liten underhållsbelastning. Att lägga samman huvudgrenen i QA-, UAT-, release- eller dev-grenar efter en snabbkorrigeringsrelease är till exempel en kostnadsfri underhållsuppgift. Ju fler paket, desto fler databaser och fler repetitiva arbetsmoment.

Använd ett verktyg som mixu/gr för att utföra rutinåtgärder på flera Git-databaser i en batch: <https://github.com/mixu/gr>

## Namnkonventioner

Med det separata paketets GRA-mönster ingår paketen i GRA-stiftelsen om det behövs för grundmetapaketet. Lägg till eller ta bort paket från metapaketet för att flytta dem in och ut från grunden.

Metapaket ger flexibilitet i paketens installationsomfång. Det är extra viktigt att förpackningarnas namn inte innehåller några ord som relaterar till den avsedda användningen av paketet. Namnet antonevers/module-gra-store-locator kan bli förvirrande när du bestämmer dig för att ta bort paketet från GRA-stiftelsen. Undvik omfattning (GRA, grund, lokal). Undvik regionen (EMEA, Spanien, Global). Du bör absolut undvika namnet på den butik som ett paket har skapats för. Välj namn som bara relaterar till de funktioner som läggs till i paketet. På så sätt kan du återanvända dem var du vill, även i oförutsedda framtida scenarier. Namnet antonevers/module-store-locator skulle vara utmärkt.

Se till att relaterade paket visas tillsammans i översikter. Skapa namn från generiska till specifika. Anonevers/module-b2b-tax-exclude istället för antonevers/tax-exclude-module-b2b.

## Exempel på koder

Kodexemplen i det här blogginlägget har kombinerats i en uppsättning Git-databaser som du kan använda för att spela upp tillsammans med konceptbeviset.

- Ett exempel på ett produktionsarkiv: <https://github.com/AntonEvers/gra-separate-brand-x>
- Ett exempel på en grundläggande modul: <https://github.com/AntonEvers/module-example-gra>
- Ett exempel på en tredjepartsmodul: <https://github.com/AntonEvers/module-example-3rdparty>
- Ett exempel på lokal modul: <https://github.com/AntonEvers/module-example-local>
- Ett exempel på grundläggande metapaket: <https://github.com/AntonEvers/gra-meta-foundation>
- Ett exempel på ett lokalt metapaket (valfritt): <https://github.com/AntonEvers/gra-meta-brand-x>
- Ett exempel på databas för disposition: <https://github.com/AntonEvers/gra-composer-repository>
