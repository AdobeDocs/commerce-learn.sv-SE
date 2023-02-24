---
title: Installera Adobe I/O Runtime kommandoradsgränssnitt och API Mesh-plugin
description: Upptäck hur du installerar Adobe I/O Runtime kommandoradsgränssnitt och API Mesh-plugin
landing-page-description: Upptäck hur du använder Adobe App Builder och installerar Adobe I/O Runtime med API Mesh-plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: f365e4cbf3f9bd8a0364acb9d28f8d9479809011
workflow-type: tm+mt
source-wordcount: '177'
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

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

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
