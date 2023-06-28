---
title: Installera Adobe I/O Runtime kommandoradsgränssnitt och API Mesh-plugin
description: Upptäck hur du installerar Adobe I/O Runtime kommandoradsgränssnitt och API Mesh-plugin
landing-page-description: Upptäck hur du använder Adobe App Builder och installerar Adobe I/O Runtime med API Mesh-plugin.
short-description: Upptäck hur du använder Adobe App Builder och installerar Adobe I/O Runtime med API Mesh-plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Installera Adobe I/O Runtime CLI- och Mesh-plugin

Innan du börjar använda API Mesh för Adobe Developer App Builder måste du installera `aio` CLI och API Mesh-plugin.
Installationsanvisningar och krav finns på API Mesh [Komma igång](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} sida.

## Vem är den här videon till?

* Utvecklare som är nya för API Mesh eller [!DNL Adobe Commerce] med begränsad erfarenhet [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} och API Mesh.

## Videoinnehåll

* Introduktion till API-nät
* Installera Adobe I/O Runtime CLI (kommandoradsgränssnitt)
* Installera API Mesh-plugin

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## Installera `aio` CLI- och API Mesh-plugin

Efter installation `node` och `npm`kör du följande kommando för att installera `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

När Adobe I/O Runtime CLI är installerat använder du följande kommando för att installera API Mesh-pluginen:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
