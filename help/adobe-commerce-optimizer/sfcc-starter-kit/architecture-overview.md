---
title: Arkitektur - översikt för Salesforce Commerce Cloud Connector
description: Läs om arkitekturen för Salesforce Commerce Cloud med Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
old-role: Architect, Developer
role: Developer
level: Beginner
doc-type: Technical Video
duration: 243
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---


# Salesforce Commerce Cloud starter kit-arkitektur

Läs om arkitekturen och funktionaliteten i Commerce Optimizer Connector Starter Kit, som integrerar Salesforce Commerce Cloud (SFCC) och Adobe App Builder. Startsatsen används av Adobe Commerce Optimizer för att effektivisera katalogsynkroniseringen för Edge Delivery-butiker. Det förklarar hur en anpassad kassett i SFCC identifierar katalogändringar via deltaexportfiler och visar dem via anpassade API:er. De här ändringarna används av App Builder runtime-åtgärder, både synkrona och asynkrona, för att utföra synkroniseringar av hela data och deltdata, metadatauppdateringar och produktspecifika synkroniseringar. Systemet innehåller också valideringsverktyg som säkerställer att förgrunden är korrekt och använder App Builder statushantering för att spåra synkroniseringsstatus och förhindra konflikter.

## Vem är den här videon till?

* Commerce Solution Architect
* Tekniker inom marknadsföring
* Administratörer för e-handelsplattform

## Videoinnehåll

* Anpassade SFCC-kassetter och API:er identifierar katalogändringar via deltaexport, vilket möjliggör effektiv datasynkronisering med Adobe App Builder.
* App Builder runtime-åtgärder hanterar fullständig synkronisering och deltasynkning samt tillståndsspårning för att säkerställa korrekta och konfliktfria uppdateringar av Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)
