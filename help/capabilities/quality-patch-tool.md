---
title: Lappa kvalitet
description: Lär dig hur du använder verktyget för kvalitetskorrigering när du diagnostiserar ett problem, söker efter en lösning och använder en korrigering som finns i den befintliga listan med tillgängliga korrigeringsfiler.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Verktyget Kvalitetskorrigering

Lär dig hur du använder verktyget för kvalitetskorrigering när du diagnostiserar ett problem, söker efter en lösning och använder en korrigering som finns i den befintliga listan med tillgängliga korrigeringsfiler.

## Vad du kommer att lära dig

Lär dig hur du löser ett problem och sedan använder du några grundläggande tekniker för att hitta en kvalitetskorrigering och tillämpa en korrigering.

## Målgrupp

* Utvecklare lär sig att hitta problem och använda det här verktyget för GIT-korrigeringar för kända problem

## Videoinnehåll

Quality Patches Tool är ett kommandoradsverktyg för Adobe Commerce och Magento Open Source. Så här kan du göra:

* Visa allmän information om de senaste kvalitetspatcharna.
* Använd högklassiga patchar i installationen.
* Återställ tillämpade korrigeringsfiler vid behov

Dessa korrigeringsfiler har utvecklats av Adobe-utvecklare i Magento Open Source community för att förbättra stabilitet och prestanda. Tänk på att du inte bör använda ett stort antal patchar eftersom det kan komplicera framtida uppgraderingar.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Varför ska jag använda verktyget för kvalitetskorrigering?

Du kan använda verktyget Kvalitetskorrigeringar för Adobe Commerce eller Magento Open Source om du vill:

Förbättra stabilitet och prestanda: Korrigeringsfiler av hög kvalitet åtgärdar problem, förbättrar säkerheten och optimerar installationen.
Håll dig uppdaterad: Genom att använda korrigeringsfiler ser du till att systemet är aktuellt och skyddat.
Återställ ändringar: Om en korrigering orsakar oväntade problem kan du återställa den med verktyget. Kom ihåg att det är bäst att använda ett begränsat antal patchar för att undvika att komplicera framtida uppgraderingar.  

## Begränsningar eller problem med att använda verktyget för kvalitetskorrigering

Det finns några fördelar med verktyget för kvalitetskorrigeringar, men det finns några saker att tänka på:

* Kompatibilitet: Kontrollera att patcharna är kompatibla med din version av Adobe Commerce eller Magento Open Source.
* Testning: Testa alltid korrigeringar i en staging-miljö innan du använder dem i produktionen. Oväntade problem kan uppstå.
* Patchberoenden: Vissa patchar kan vara beroende av andra. Var uppmärksam på eventuella förutsättningar.
* Anpassningar: Om du har gjort anpassade kodändringar kan det uppstå konflikter för korrigeringsfiler. Granska ändringarna noggrant.
* Säkerhetskopiera: Säkerhetskopiera installationen innan du använder korrigeringsfiler för att undvika dataförlust.

Verktyget Kvalitetspatchar är användbart för ett begränsat antal patchar, men rekommenderas inte för hantering av stora mängder patchar. Om du använder för många patchar kan det komplicera framtida uppgraderingar och underhåll. Om du har många patchar att använda bör du överväga alternativa lösningar eller rådfråga en Magento-specialist. 

## Sammanfattning

Med verktyget Quality Patches Tool kan e-handelsplattformar förbättra stabilitet och säkerhet genom att använda korrigeringsfiler. Dessa korrigeringsfiler åtgärdar problem, förbättrar prestanda och optimerar systemet. Att hålla installationen uppdaterad garanterar skydd mot sårbarheter.

Innan du använder korrigeringsfiler är det viktigt att testa dem i en staging-miljö. Säkerställ kompatibiliteten med din specifika version av Adobe Commerce eller Magento Open Source. Vissa korrigeringsfiler kan ha beroenden, så kontrollera förutsättningarna noggrant.

 Säkerhetskopiera installationen innan du använder korrigeringsfiler för att förhindra dataförlust. Om du har gjort egna kodändringar bör du vara medveten om att det kan uppstå konflikter mellan korrigeringsfiler. Följ bästa praxis och övervaka effekten av varje korrigering.

## Relaterade artiklar och videoklipp

* [Sökning i kvalitetskorrigeringsverktyg](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [Versionsinformation](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github för patchar](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Användning av kvalitetskorrigeringsverktyget](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Teknisk video på QPT](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)
