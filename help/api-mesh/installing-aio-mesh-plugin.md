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
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Installera Adobe I/O Runtime CLI- och Mesh-plugin

Innan du börjar använda API Mesh för Adobe Developer App Builder måste du installera CLI:n `aio` och API Mesh-pluginen.
Installationsanvisningar och krav finns på sidan [Komma igång](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} för API-nät.

## Vem är den här videon till?

* Utvecklare som är nybörjare i API-nät eller [!DNL Adobe Commerce] med begränsad erfarenhet av att använda [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} och API-nät.

## Videoinnehåll

* Introduktion till API-nät
* Installera Adobe I/O Runtime CLI (kommandoradsgränssnitt)
* Installera API Mesh-plugin

>[!VIDEO](https://video.tv.adobe.com/v/3430770?captions=swe&quality=12&learn=on)

## Installerar plugin-programmet `aio` för CLI och API Mesh

När du har installerat `node` och `npm` kör du följande kommando för att installera CLI:n för `aio`:

```bash
npm install -g @adobe/aio-cli
```

När Adobe I/O Runtime CLI är installerat använder du följande kommando för att installera API Mesh-pluginen:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
