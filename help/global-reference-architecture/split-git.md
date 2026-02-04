---
title: Konfigurera Adobe Commerce med den delade Git Global Reference Architecture
description: Lär dig hur du konfigurerar Adobe Commerce med Split Git Global Reference Architecture för effektiv kodhantering och effektiv driftsättning. ​
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Medverkad av Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Medverkad av Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Det globala referensarkitekturmönstret för Split Git

{{only-for-on-prem-commerce-cloud}}

I den här guiden beskrivs hur du konfigurerar Adobe Commerce med GRA-mönstret (Split Git Global Reference Architecture).

Det delade Git GRA-mönstret innefattar två Git-databaser för utveckling och en Git-databas per Adobe Commerce-instans. I exemplen antas att varje instans representerar ett unikt varumärke.

![Ett diagram som visar var kod lagras i ett delat GRA-mönster](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## Fördelar och nackdelar med mönstret

Fördelar:

- Kodåteranvändning via ett delat kodarkiv
- Enkelt GRA-mönster, lämpligt även för team med begränsade dispositionskunskaper
- Förutom Adobe Commerce-moduler, teman och språkpaket går det att installera alla typer av Composer-paket via den här modellen, inklusive Composer-plugin, Composer-metapackage, magento2-komponent och patchar
- Möjligt att släppa i faser, planera releaser för regioner i sina egna underhållsperioder
- Stöd för Git-taggar i administrationssyfte, inte för distributionskontroll
- Garantera att kombinationen av paket i en produktionsdistribution utvecklas och testas i just den här konfigurationen

Nackdelar:

- Ingen flexibilitet jämfört med andra GRA-mönster
- Det går inte att uppgradera eller nedgradera enskilda moduler per instans. Uppgradera eller nedgradera alltid GRA som helhet
- I de flesta fall passar bulkpaketmönstret bättre eftersom det är lika enkelt, men mer konventionellt

## Konfigurera Adobe Commerce med GRA-mönstret för delat Git

### Katalogstrukturen

Det delade Git GRA-mönstret har två typer av databaser: utvecklingsdatabaser och installationsdatabaser. Utvecklingsdatabaserna innehåller bara en del av en fullständig Adobe Commerce-installation. Installationsdatabaserna innehåller den fullständiga Adobe Commerce-installationen och används för distribution, men inte för utveckling.

Den slutliga katalogstrukturen för en fullständig Adobe Commerce-installation med GRA-mönstret Split Git ser ut så här:

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Katalogerna `app/code`, `app/i18n` och `app/design` har utelämnats eftersom Composer inte utvärderar kod i dessa kataloger. Därför installeras inte beroenden som deklareras i paketen automatiskt. Det delade Git GRA-mönstret löser det här problemet genom att installera all anpassad kod i `packages/` och behandla den katalogen som en dispositionsdatabas. Kompositör-symbolpaket inuti `packages/` till `vendor/`.

### Förbereda Git-databaser

Skapa 3 Git-databaser för:

1. En Adobe Commerce-instans
2. Tredjepartskod som inte installeras via Composer
3. Dina anpassningar i form av moduler, teman, språkpaket och liknande, din GRA

I den här vägledningen används följande namn för dessa databaser:

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

Alla databaser i GRA-mönstret för delad Git sammanfogas till en. För att Git ska kunna sammanfoga flera databaser behöver alla tre databaserna en delad historik. Skapa ett Git-projekt med en enda implementering och skicka det till alla fjärrplatser.

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

När den temporära filen `.gitkeep` skickas till alla fjärrplatser skapas samma initiala implementering med samma implementeringshash, vilket skapar en delad historik. Varje ändring som skapas i en fjärranslutning kan sammanfogas med de andra.

Här skiljer sig databaserna åt. Databasen gra-split-brand-x innehåller varumärkesspecifik kod. Databasen gra-split-3rdparty innehåller endast kod från tredje part. Databasen gra-split-gra innehåller endast den globala referensarkitekturbasen, som består av all egen kod.

Installera Adobe Commerce i grå-split-brand-x-databasen.

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

Skapa initiala implementeringar i grå-delad-3-partsdatabaser och grå-delad-grå-databaser. Det enklaste sättet är att checka ut dessa databaser i separata kataloger.

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

Dessa två databaser lagrar paket från tredje part och GRA-paket. Det kan finnas kod som är exklusiv för varje instans av Adobe Commerce. Skapa en plats där du kan lagra dessa lokala paket i databasen gra-split-brand-x.

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### Var olika typer av kod ska lagras

Adobe Commerce är ett Composer-program. Det bästa sättet att installera är alltid via Composer-databaser. Endast om en modulleverantör inte erbjuder installation via en Composer-databas kan du lagra tredjepartsmoduler i tredjepartsdatabasen. Den anpassade koden ska helst placeras i GRA-databasen. När en modul bara används av en viss instans, blir den lokal kod.

Sammanfattning:

- **Adobe Commerce**: lagras i en Composer-databas.
- **Tredjepartsmoduler**: lagras i en Composer-databas.
- **Alternativet för reservmoduler från tredje part**: lagras i Git-databasen gra-split-3rdparty.
- **GRA-stiftskod**: lagras i Git-databasen gra-split-gra.
- **Lokal kod**: lagras i Git-databasen gra-split-brand-x.

### Anslut paketlagring till Composer

Composer kan hantera paketkatalogen som en dispositionsdatabas. Informera Composer om platsen för paket i paketkatalogen.

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer söker efter filen Composer.json på två nivåer i de tre lagringskatalogerna. Skapa underkataloger i de tre kodlagringskatalogerna exakt som de skulle visas i katalogen `vendor/`.

Exempel: Om ett paket normalt installeras i `vendor/example-corp/module-example/` lagrar du det i `packages/3rdparty/example-corp/module-example/`. Composer länkar paketet till `vendor/example-corp/module-example/` när du behöver det.

Använd Composer-paketets namnutrymme och namn som katalogstruktur. En modul som traditionellt finns i `app/code/MyCorp/MyCustomization/` har namnet `my-corp/module-my-customization` i Composer.json. Lagra paketet i `packages/gra/my-corp/module-my-customization`.

### Inkludera nya paket i instansdatabaserna

Sammanfoga paket från tredje part och GRA-fjärrkontroller till databasen gra-split-brand-x.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

Resultatet är följande katalogstruktur:

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Förändringarna i databasen från tredje part och GRA Foundation sammanförs i varumärkesarkiven. På så sätt upprätthålls endast tredjeparts- och GRA-kod på ett ställe. Flytta ändringarna till varumärkena med en Git-sammanslagning.

Adobe Commerce känner inte igen nya moduler automatiskt. Kör disposition kräver att ett nytt paket läggs till efter en sammanfogning. Kör uppdatering av dispositionen varje gång du uppdaterar ett av paketen efter en sammanslagning.

### Installera exempelmoduler

Som ett konceptbevis kan du installera exempelmoduler för att se hur GRA-mönstret fungerar.

Kör `composer install` och `bin/magento install` innan du fortsätter.

Det finns tre testmoduler för GitHub:

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### Installera en lokal modul

Det är enkelt att lägga till en modul i den lokala kodpoolen. Hämta och extrahera modulen. Kräv det med Composer. Aktivera det med `bin/magento` och implementera filerna i varumärkesdatabasen.

```bash
cd gra-split-brand-x
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

Om du ser utdata ovan kan du implementera dem i varumärkesdatabasen på ett säkert sätt.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### Installera och utveckla en GRA-modul

Att lägga till en modul i GRA-databasen skiljer sig från att installera lokala moduler. Som standard läggs implementeringar till i origin/main, som är grå-split-brand-x-databasen. Förändringar av GRA-moduler bör föras över till databasen gra-split-gra och därefter slås samman i databasen gra-split-brand-x.

### Skapa en utvecklingsmiljö

Skapa en utvecklingsmiljö med en kombination av alla kodpooler på ett och samma ställe. Du kan skicka kod till den lokala databasen, GRA-databasen och tredjepartsdatabasen individuellt via symboler. Börja med att skapa en ny utvecklingskatalog bredvid era varumärkeskataloger, GRA-kataloger och tredjepartskataloger.

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

Resultatet är följande katalogstruktur:

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

Kör `composer install` och `bin/magento install` i katalogen för grå utveckling.

Det är nu möjligt att genomföra ändringar direkt från katalogerna `packages/3rdparty`, `packages/gra` och `package/local`. Git implementerar ändringarna i Git-databasen som katalogerna länkar till. Det är viktigt att Git-kommandot implementeras skickas inuti katalogen `packages/3rdparty`, `packages/gra` eller `package/local`. Kör inte Git-implementering vid projektets rot.

### Installera exempelmoduler

Installera exempelmodulerna från tredje part och GRA i paketkatalogerna.

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

Det sista kommandot bör resultera i följande utdata som visar att modulen är installerad och fungerar:

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

Om du ser utdata ovan kan du implementera dem i varumärkesdatabasen på ett säkert sätt. Kör `git remote -v` för att verifiera att du har implementerat på rätt fjärr.

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### Leverera kod till förekomsterna

Sammanfoga GRA- och tredjepartsdatabaserna till databasen gra-split-brand-x för att leverera koden till en Adobe Commerce-instans. Kör `composer require`, `bin/magento module:enable` och bekräfta resultatet.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## Förgreningsstrategi

Det här GRA-mönstret fungerar med alla förgreningsstrategier, om du speglar förgreningsstrategin för butiksdatabaserna i din tredjeparts- och GRA-databas. För releaser skapar du en officiell gren med samma namn i alla tre databaserna. Sammanfoga de frisläppta grenarna i butiksarkivet när du förbereder releasen.

Ibland har du en biljettgren som kräver att både lokal kod och tredjepartskod eller GRA-kod ändras. I det här fallet måste biljettgrenarna skapas i alla relaterade databaser.

Sammanfoga aldrig utfästelser från tredje part och GRA-utfästelser i varumärkesarkivet inuti biljettfilialer. Ta istället en titt på rätt grenar i utvecklingsmiljön för varje kodpool. Det går bara att sammanfoga i varumärkesdatabasen när releasen disponeras eller när en QA-gren disponeras.

## Exempel på koder

Kodexemplen i den här artikeln är tillgängliga som en uppsättning Git-databaser, som du kan använda för att testa konceptbeviset.

- Ett exempel på ett produktionsarkiv: <https://github.com/AntonEvers/gra-split-brand-x>
- Tredjepartskoddatabasen: <https://github.com/AntonEvers/gra-split-3rdparty>
- GRA-koddatabasen: <https://github.com/AntonEvers/gra-split-gra>
- Ett exempel på lokal modul: <https://github.com/AntonEvers/module-example-local>
- Ett exempel på GRA-modul: <https://github.com/AntonEvers/module-example-gra>
- Ett exempel på en tredjepartsmodul: <https://github.com/AntonEvers/module-example-3rdparty>
