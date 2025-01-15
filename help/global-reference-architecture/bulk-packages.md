---
title: Optimera Adobe Commerce med Global referensarkitektur för grupppaket
description: Lär dig hur du konfigurerar Adobe Commerce med Bulk Packages Global Reference Architecture för effektiv kodhantering och versionskontroll.
kt: 16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Medverkad av Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Medverkad av Tony Evers"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# Det globala referensarkitekturmönstret för masspaket

I den här guiden beskrivs hur du konfigurerar Adobe Commerce med GRA-mönstret (Bulk Packages Global Reference Architecture).

GRA-mönstret för grupppaket innehåller en enda Git-databas som innehåller alla vanliga anpassningar. Denna enda Git-databas visas via Composer som ett enda dispositionspaket, som innehåller flera Adobe Commerce-moduler.

![Ett diagram som visar var kod lagras i ett GRA-mönster för grupppaket](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Fördelar och nackdelar med mönstret

Fördelar:

- Kodåteranvändning via ett delat kodarkiv
- Flexibilitet att installera olika historiska versioner av GRA på olika instanser, vilket möjliggör fasbaserade versioner
- Flexibilitet att stödja och underhålla flera större versioner av GRA
- Stöd för semantisk versionshantering av GRA
- Enkelt uttryckt behöver utvecklare inte mer kompetens än i vanliga utvecklingsmönster för en enda butik
- Inga särskilda verktyg, komplex infrastruktur eller särskilda förgrenade strategier krävs
- Kombinationen av paket i en release utvecklas och testas alltid tillsammans

Nackdelar:

- Det är bara möjligt att uppgradera hela GRA, inklusive alla paket som ingår i det.
- Inget stöd i GRA-paketet (bulk package) för andra dispositionspaket än Adobe Commerce-moduler, språkpaket och teman, så inga metapaket, magento2-komponentpaket, Composer-plugin-program och patchar

## Konfigurera Adobe Commerce med GRA-mönstret för delat Git

### Katalogstrukturen

GRA för masspaket installerar all återanvändbar kod via Composer-databaser. Kod som är specifik för en enskild instans finns inuti den instansens Git-databas. Instansspecifik kod återanvänds inte i andra instanser av Adobe Commerce.

Den slutliga katalogstrukturen för en fullständig Adobe Commerce-installation med GRA-mönstret för grupppaket ser ut så här:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Katalogerna `app/code`, `app/i18n` och `app/design` har utelämnats eftersom Composer inte utvärderar kod i dessa kataloger. Därför installeras inte beroenden som deklareras i paketen automatiskt. GRA-mönstret för grupppaket löser problemet genom att installera anpassad kod i `packages/` och behandla katalogen som en dispositionsdatabas. Kompositör-symbolpaket inuti `packages/` till `vendor/`.

### Förbereda Git-databaser

Skapa två Git-databaser för den delade GRA-koden och för den första lagringsplatsen. Börja med GRA-databasen som har följande filstruktur:

Resultatet är följande katalogstruktur:

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

I den här katalogstrukturen nämns bara filen Composer.json och filen registration.php för modulerna GraOne och GraTwo. I verkligheten finns det fler filer i de här modulerna.

Kör dessa kommandon för att initiera Git-databasen:

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

Innehållet i filen Composer.json:

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

Ändra namnutrymmet för Composer.json-paketet till ditt eget namnutrymme.

Innehållet i src/registration.php:

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

Filen registration.php söker efter andra registration.php-filer i Adobe Commerce-modulerna och kör dem.

Skapa de två exempelmodulerna med koden i <https://github.com/AntonEvers/gra-bulk-foundation>. Composer utvärderar inte filen Composer.json i exempelmodulerna. De är där som en vana. Om du bestämmer dig för att flytta modulerna till en annan plats blir filen Composer.json nödvändig igen.

### Konfigurera arkivdatabasen

Distributionsdatabasen innehåller hela Adobe Commerce-installationen inklusive GRA-koden. Skapa distributionsdatabasen:

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installera Adobe Commerce med `bin/magento setup:install`. Installera GRA-exempelmodulerna i distributionsdatabasen med Composer:

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

Det sista kommandot bör resultera i följande utdata som visar att modulen är installerad och fungerar:

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

Du kan skapa flera grupppaket för att organisera koden. Ett tredjepartspaket för tredjepartskod som inte är tillgängligt via Composer. Allt som du traditionellt skulle installera i `app/code` bör nu finnas i katalogen `src/` i grupppaketet. Ett undantag till den regeln är kod som bara används i en enda instans. Dessa paket kallas lokala paket.

### Installera lokala paket

Distributionsdatabasen är värd för lokala paket. De bor inte i GRA-paketet. Platsen för de lokala paketen är inte `app/code` utan `packages/local`. Instruera Composer att behandla den här katalogen som en databas:

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

Lägg till exempelmodulen som finns på <https://github.com/AntonEvers/module-example-local>:

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

Det sista kommandot bör resultera i följande utdata som visar att modulen är installerad och fungerar:

```bash
Local module is installed successfully and working!
```

Bekräfta den lokala modulen i varumärkesdatabasen:

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## Översikt över kodplatser

Endast om den tredje parten inte erbjuder installation via en Composer-databas kan du lagra tredjepartsmoduler i katalogen `src/` i din grunddatabas eller ett dedikerat tredjepartspaket.

- **Adobe Commerce Core**: tillgängligt via repo.magento.com.
- **Tredjepartsmoduler**: tillgängliga via Marketplace eller en leverantörs egen Composer-databas.
- **Alternativet för reservmoduler från tredje part**: lagras i `src/` för ett grupppaket.
- **GRA-stiftskod**: lagras i `src/` för grundbulkpaketet.
- **Lokal kod**: lagras i katalogen `packages/local` i distributionsdatabasen.

## Utveckla en GRA-modul

Installera grupppaketet från källan för att aktivera Git i grupppaketets katalog:

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

Grupppaketet har checkats ut med Git. När du anger katalogen `vendor/antonevers/gra-bulk-foundation` anger du även Git-databasen gra-bulk-foundation. Du kan skapa, checka ut och sammanfoga grenar i den här katalogen.

Lägg till Composer-beroenden i filen Composer.json i roten för GRA-bulkpaketet, som är den enda filen i bulkpaketet som utvärderas i Composer.

## Inkludera tredjepartsmoduler i GRA-bulkpaketet

Lägg till tredjepartspaket i avsnittet require i Composer.json i roten av GRA-stiftelsen för att lägga till dem i GRA-filen. På så sätt installeras paketen alltid i alla instanser via Composer.

## Leverera din kod

Det finns två sätt att leverera kod till huvudgrenen. Först de lokala modulerna, som sammanfogas med huvudgrenen. Kör Composer-uppdatering för de modulerna. Tillåt inte att utvecklare uppdaterar Composer.lock i sina biljettgrenar för att minska konflikterna. Uppdatera bara filen Composer.lock i mellanlagrings- och produktionsgrenar, vilket minskar risken för konflikter.

För det andra, GRA-bulkpaketen, som slås samman till huvudgrenen i GRA:s bulkdatabas. Sedan kan du lägga till en Git-tagg i huvudgrenen och versionshantera Composer-paketet. Kräv att du har en ny version av GRA-bulkpaketet i Composer.json i distributionsdatabasen för att installera det.

## Förgreningsstrategi

Detta GRA-mönster fungerar med alla förgreningsstrategier så länge du speglar förgreningsstrategin för distributionsdatabaserna i din GRA-bulkdatabas. För releaser skapar du en frisläppningsgren med samma namn i båda databaserna. Skapa en biljettgren i båda databaserna för att kunna utveckla.

I biljettgrenar behöver du nästan aldrig uppdatera filen disposition.lock. Ta en titt på rätt grenar i utvecklingsmiljön för både butiken och GRA Foundation-databasen med Git. Undantaget är när du uppdaterar kraven i filen GRA Foundation Composer.json. Uppgradering av GRA-grunden i distributionsdatabasen görs endast när releasen byggs, eller när en QA-gren skapas.

## Exempel på koder

Kodexemplen i den här artikeln är tillgängliga som en uppsättning Git-databaser, som du kan använda för att testa konceptbeviset.

- Ett exempel på ett produktionsarkiv: <https://github.com/AntonEvers/gra-bulk-brand-x>
- GRA-koddatabasen: <https://github.com/AntonEvers/gra-bulk-foundation>
- Ett exempel på lokal modul: <https://github.com/AntonEvers/module-example-local>
