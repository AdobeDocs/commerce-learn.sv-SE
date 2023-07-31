---
title: Lär dig hur cachning för mysql-frågor
description: Ibland backas mysql-frågor upp i väntan på ett lås. I den här självstudiekursen förklaras vad som är frågecachelagring och några rekommendationer för inställningar om du har problem.
kt: 13690
doc-type: video
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: d62f409324eda53ca42cf2e9c9aa80655a1277d7
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# Lär dig mer om cachning av mysql-frågor

Lär dig vad MySQL-frågecachen är och lite grundläggande förståelse för hur den fungerar. Lär dig hur du upptäcker ett problem med cache-lagring av mysql-frågor genom att hitta&quot;Väntar på cache-lås för fråga&quot; i en hög volym i mysql slow query logs-loggarna.

## Vem är den här videon till?

- Arkitekter
- Utvecklare
- DevOps

## Videoinnehåll

- Läs mer om frågecachelagring
- Så här identifierar du om inställningarna för frågecachen kan vara problem genom att söka efter&quot;Väntar på cache-lås för fråga&quot;
- Se hur SQL sparas och används för att hitta en matchande frågecache
- Några tips om konfigurationsinställningar

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## Användbara resurser

- [Allmänna riktlinjer för MySQL](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=en){target="_blank"}
- [Deadlocks in MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}
- [Galerareplikering och långsamma frågor](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html){target="_blank"}
