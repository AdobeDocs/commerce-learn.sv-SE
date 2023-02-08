---
title: Installerar kommandoradsgränssnitt för Adobe Developer IO och API Mesh-plugin
description: Upptäck hur du installerar kommandoradsgränssnittet för Adobe Developer IO och API Mesh-plugin
landing-page-description: Upptäck hur du använder Adobe App Builder och installerar Adobe Developer IO med API Mesh-plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---


# Installerar Adobe Developer IO- och Mesh-plugin

Innan du börjar finns det några saker som behöver konfigureras. Först Adobe Developer IO kommandoradsgränssnitt. Kontrollera sedan att API Mesh-plugin-programmet är konfigurerat i varje miljö.
Instruktioner om hur du konfigurerar din lokala miljö för att köra Node, nvm och installera Adobe Developer IO finns på [Komma igång med GraphQL Mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## Vem är den här videon till?

* Utvecklare som är nybörjare i Adobe App Builder eller [!DNL Magento Open Source] med begränsad erfarenhet av Adobe Developer IO och API Mesh.

## Videoinnehåll

* Introduktion till API-nät
* Installera kommandoradsgränssnittet för Adobe Developer IO
* Lägga till API Mesh-plugin-programmet på AIO-kommandoraden

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## Exempelkommandon med NPM och AIO

Det är ganska enkelt att installera Adobe Developer kommandoradsgränssnitt. Kör det här kommandot när du har installerat Node `npm install -g @adobe/aio-cli`
När Adobe Developer-klippet har installerats går det att installera plugin-programmet för nät. Du gör detta genom att köra det här kommandot `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
