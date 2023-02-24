---
title: Skapa en GraphQL med flera källor som ska användas i API-nät
description: Upptäck hur du kan använda flera källor för API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig mer om några vanliga fel och hur du löser dem.
landing-page-description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar ett nät som har flera källor och hur du löser några vanliga fel.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 6b2a66ed3b4de8e633d5b3c6bce86eb0507a9ac7
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# Skapa ett nät med flera källor

Den här videon hjälper utvecklare att förstå hur man skapar ett nät med flera källor i API Mesh för Adobe Developer App Builder. I den här videon visas hur du skapar ett nät med flera källor och identifierar fel. Mer information och kodexempel finns på [Skapa ett nät](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## Vem är den här videon till?

* Alla som är nybörjare i API-nät
* Utvecklare som är intresserade av att kombinera flera API- och GraphQL-källor

## Videoinnehåll

* Så här använder du [omformningar](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} för att ändra standardkällschemat
* Felsöka fel som namnkonflikter, schematillgänglighet och andra schemasyntaxproblem
* Uppdatera ditt nät med en ändrad konfiguration

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Skapa JSON-konfigurationsfilen

API Mesh använder en JSON-konfigurationsfil för att definiera dina källhanterare. JSON-filen innehåller en `sources` -array som innehåller källor för ditt nät. Här är ett exempel på ett nät med flera källor.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
