---
title: Lär dig hur Percona Toolkit pt-query-digest fungerar och varför det används
description: Analysera MySQL-frågor från långsamma, allmänna och binära loggfiler. Den kan också analysera frågor från "SHOW PROCESSLIST" och MySQL-protokolldata från tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Lär dig varför du använder pt-query-digest och några verkliga exempel för att fördjupa resonemanget.

## Vem är den här videon till?

- Arkitekter
- Utvecklare
- DevOps

## Videoinnehåll

- Läs mer om användningen av pt-query-digest
- Lär dig mer om fördelarna och bristerna med den här Percona Toolkit-funktionen
- Förstå resultaten och lär dig vilka möjliga prestandasteg som ska övervägas

>[!VIDEO](https://video.tv.adobe.com/v/3452296?captions=swe&learn=on)

## Kodreferenser

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Användbara resurser

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
