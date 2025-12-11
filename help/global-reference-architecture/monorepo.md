---
title: Global Reference Architecture Monorepo
description: Lär dig använda monorepo-metoden för global referensarkitektur för att skapa en skalbar och flexibel e-handelsupplevelse
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Medverkad av Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Medverkad av Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---

# Monorepo Global Reference Architecture pattern

I den här guiden beskrivs hur du konfigurerar Adobe Commerce med GRA-mönstret (Monorepo Global Reference Architecture).

Monorepo GRA-mönstret innehåller en enda Git-databas som innehåller alla vanliga anpassningar. Denna enda Git-databas visas via Composer som ett separat dispositionspaket.

![Ett diagram som visar var kod lagras i ett GRA-mönster för monorepo ](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Fördelar och nackdelar med mönstret

Fördelar:

- Idealiskt för funktionstestning
- Kod återanvänds via delade koddatabaser
- Fullständig flexibilitet vid paketinstallation - varje GRA-paket kan uppgraderas, nedgraderas eller backporteras individuellt
- Fullt stöd för semantisk versionshantering
- Inga särskilda verktyg, komplex infrastruktur eller särskilda förgrenade strategier krävs
- Stöd för alla pakettyper som Composer stöder
- Perfekt för tillfälliga miljöer som är valfria, men för team med stora volymer är de mycket användbara

Nackdelar:

- Möjlighet att distribuera kombinationer av paket som inte har utvecklats i samma konfiguration, kräver strikta testprocedurer
- GRA-mönstret för monorepo kan vara komplext från början. Tilldela ett lead som hjälper teamet att arbeta med systemet

## Konfigurera Adobe Commerce med GRA-mönstret för separata paket

### Katalogstrukturen

Den sista katalogstrukturen i en fullständig Adobe Commerce-installation med GRA-mönstret för separata paket har följande katalogstruktur:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

En Git-databas för produktion har den här katalogstrukturen:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

Skillnaden är att produktionsinstanserna installeras från Composer, där monorepon har en egen kopia av varje paket i paketkatalogen.

### Förbereda en produktionsdatabas

Skapa en databas för den första Adobe Commerce-instansen, som representerar en webbutik för Brand X.

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installera Adobe Commerce med `bin/magento setup:install`. Genomför de resulterande `app/etc/config.php`- och dispositionsfilerna. Composer hanterar allt annat så att inget annat ska vara i Git.

### Förbered monorepo-databasen

Databasen i monorepo börjar med samma steg.

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

Installera Adobe Commerce med `bin/magento setup:install`, implementera och push.

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### Förbered dig för monorepo-utveckling

För monorepo-utveckling gör du följande ändringar i filen Composer.json:

1. Ändra paketets namn och beskrivning så att det är tydligt att det här paketet är ditt monorepo-paket.
1. Ta bort versionsattributet från Composer.json, eftersom versionen hanteras med Git-taggar för den här databasen.
1. Ersätt kravavsnittet med ett metapaket som skapas senare.
1. Ändra minsta stabilitet till dev.
1. Lägg till en sökvägstypsdatabas som pekar på `packages/*/*` som värd för monorepopaket, inklusive filialalias för varje paket som den innehåller
1. Lägg till ett grenalias för själva projektet

I följande Git-diff visas skillnaden mellan en ren Adobe Commerce-installation och de ändringar som nämns ovan:

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### Använd metapaket

Hämta exempelkoden från [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) på GitHub för att hämta metapaketen och exempelmodulerna som används i det här exemplet.

Composer-metapaket paketerar flera dispositionsprogram tillsammans i ett enda paket. När ett metapaket behövs installeras alla paket som paketeras automatiskt via Composer, vilket kräver att du använder en del av metapaketet.

I det här exemplet finns det två metapaket:

1. **antonevers/gra-meta-brand-x**: Ett metapaket som innehåller allt som innehåller &quot;Brand X&quot;
1. **antonevers/gra-meta-foundation**: Ett metapaket som innehåller allt som alltid är installerat i alla varumärken

Varumärkesmetapaketet kräver grundmetapaketet. När det krävs ett metapaket för varumärket krävs också basmetapaketet automatiskt. Se de två filen Composer.json i metapaketen för att se hur de relaterar:

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

Metapaket är ett bra sätt att ordna kod som hör ihop. Använd metapaket för att definiera grupper av paket som är regionala, globala, varumärkesspecifika eller valfri gruppering som passar dig. Om du har flera installationer av Adobe Commerce är det ett säkert och mångsidigt sätt att definiera i vilket sammanhang ett paket förväntas.

Det finns metapaket i monorepo i katalogen `packages`. Där speglas katalogstrukturen för `vendor`:

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### Lägga till och utveckla moduler

Moduler i monorepo finns i katalogen `packages`. På så sätt kan Composer hitta dem via sökvägstypsdatabasen.

Hämta exempelkoden från [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) på GitHub för att hämta metapaketen och exempelmodulerna som används i det här exemplet.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

Du kan ha flera namnutrymmen i katalogen `packages` om det behövs.

Utveckling äger rum i paketkatalogen. Symboler till paketen i katalogen `packages` skapas i katalogen `vendor` när du kör `composer update`. På så sätt blir koden en del av Adobe Commerce-installationen.

Kör `bin/magento module:enable --all` eller bara för specifika moduler för att aktivera de moduler som lagts till.

Nu bör du ha en fungerande Adobe Commerce-installation med de tre exempelmodulerna installerade och fungerar. Du kan validera om modulerna är installerade och fungerar genom att köra kommandona:

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### Skapa automatiska paket

Det finns flera sätt att skapa automatiska paket. Vissa alternativ är:

1. [Privat Packagist](https://packagist.com/)
1. [Förenkla Monorepo Builder](https://github.com/symplify/monorepo-builder)
1. Skapa en egen lösning

[Privat Packagist](https://packagist.com/) automatiserar identifieringen av paket i Git-monorepo och visar dem via Composer. Den är kompatibel med Adobe Commerce, snabb, låg underhållstid och felbenägen, vilket är anledningen till att den här guiden fokuserar på alternativet Privat paketering.

Den här guiden förklarar hur du konfigurerar privat paketering. Se [docs](https://packagist.com/docs).

Du kan göra om ett paket till ett monorepo när du har konfigurerat organisationssynkronisering och dina Git-databaser automatiskt synkroniseras till en privat Packagist.

Gå först till fliken Paket och hitta monorepon:

![Privat paketeringsskärm som tagits med monopostpaketet synligt på paketskärmen](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

Klicka på monorepopaketet och klicka på&quot;Redigera&quot; på informationsskärmen, som tar dig till följande sida:

![Privat paketeringsskärm som tagits med redigeringssidan för monopostpaket](/help/assets/global-reference-architecture/packagist-packages-edit.png)

Under det första inmatningsfältet finns en länk som säger: Skapa en databas med flera paket. Klicka på länken.

![Privat paketeringsskärm med konfiguration för flera paket](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Definiera den plats där kompositörpaket kan hittas inuti ditt monorepo. I exemplet är platsen `packages/**/composer.json`. Ändra platsen för att begränsa eller bredda var privata paket söker efter paket som ska extraheras.

På fliken Paket visas alla hittade paket när de har sparats och själva monorepon visas inte längre som ett Composer-paket:

![Privat paketeringsskärm med alla monorepo-paket synliga på paketskärmen](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

En version skapas i Composer för varje paket i monorepo, för varje tagg eller gren som skapas i monorepon i Git.

## Installera paketen i produktionsmiljön

Följ instruktionerna från Privat Packagist för att lägga till Privat Packagist som en dispositionsdatabas. Privata Packagist kan och bör användas som en spegling för alla Composer-databaser och Git-databaser, inklusive packagist.org. På så sätt behöver du inte dela inloggningsuppgifter med utvecklare och du har fullständig kontroll över varje paket. Det här exemplet följer inte denna bästa praxis eftersom det skulle visa Adobe Commerce-kodbasen offentligt.

Hämta [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) från GitHub om du vill se ett exempel på en produktionsbutik.

Det finns ingen `packages`-katalog i produktionsarkivet och alla paket installeras via Composer. Det enda paket som krävs är:

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

Alla Adobe Commerce och hela GRA installeras dock genom metapaketets krav.

## Versioner

Alla paket i monorepo får samma version som monorepo. Tänk på det som att publicera nya versioner av hela programmet. I produktionen kan du dock installera en blandning av paket från olika monorepo-versioner.

## Eleganta miljöer

Om du använder tillfälliga miljöer eller tänker använda dem är monorepo ett utmärkt val. Varje version och gren av monorepo innehåller alla Adobe Commerce-, tredjeparts- och anpassade modulfiler. Med en fullständig installation i varje gren är det möjligt att köra alla typer av tester, inklusive funktionstester. Med andra GRA-inställningar, som separata paket eller grupppaket GRA, måste du först skapa en fungerande Adobe Commerce-miljö innan du kan köra funktionstester. Från DevOps-perspektivet gör monorepo det mycket enklare.

## Exempel på koder

Kodexemplen i den här artikeln har kombinerats i en uppsättning Git-databaser som du kan använda för att spela upp tillsammans med konceptbeviset.

- Ett exempel på monorepo-databas: <https://github.com/AntonEvers/gra-monorepo>
- Ett exempel på ett produktionsarkiv: <https://github.com/AntonEvers/gra-monorepo-brand-x>
