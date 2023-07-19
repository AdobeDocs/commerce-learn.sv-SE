---
title: Lär dig hur du hittar långsamma frågor i mysql slow query logs-loggar och varför designmetoden för Galera DB-replikering kan vara orsaken
description: Galera DB har en designmetod som gör att replikering av data till sekundära databaser tar längre tid än den primära. Lär dig hur du hittar dessa händelser i mysql slow query log, och den underliggande orsaken till varför du ser poster i långsamma frågeloggar och kanske hur du förhindrar dem i framtiden.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: ae85993d98fb8620b763a212e5a2d7e53596870f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# Lär dig mer om Galera DB-replikering och relaterade långsamma MySQL-frågor

Galera-kluster hjälper till med prestanda och skalbarhet. När det gäller sekundära databaser är det viktigt att förstå hur datareplikeringen sker på ett annat sätt än på den primära. Den primära databasen kan utföra gruppåtgärder. När replikeringen utförs för alla sekundära databaser utförs en åtgärd åt gången. Om du t.ex. har 67 000 000 objekt i en borttagning sker en i taget i de sekundära databaserna. När du granskar de långsamma frågeloggarna i Mysql kan den här åtgärden ta lång tid. Eftersom de sekundära databaserna utför saker en i taget är det en anledning till att saker inte är synkroniserade och prestandapåverkan kan upptäckas.

Som en lösning kan du om möjligt batchbearbeta stora åtgärder så att de sekundära databaserna synkroniseras med den primära. Genom att göra saker i grupp kan åtgärderna utföras i rätt tid och prestandaeffekterna hålls nere till ett minimum.

## Vem är den här videon till?

- Arkitekter
- Utvecklare
- DevOps

## Videoinnehåll

- Galerareplikering till sekundär databas
- Läs om flödeskontroll
- Söker efter trådnummer i mysql slow query logs
- Massexekveringar utförs bara i huvudprogrammet. Replikeringar sker 1 i taget
- Gruppera dina stora implementeringar för att hjälpa replikeringen att hålla jämna steg med den primära

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## Användbara resurser

- [Galera Cluster](https://galeracluster.com/)
