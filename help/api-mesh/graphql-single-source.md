---
title: Skapa ett GraphQL-nät med en enda källa i API-nät
description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar ett nät som har en källa.
landing-page-description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar ett nät som har en källa.
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has one source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Skapa ett nät med en enda källa

Den här videon hjälper utvecklare att förstå hur man skapar ett nät med en enda källa i API Mesh för Adobe Developer App Builder. För att det här grundläggande exemplet ska fungera som förväntat behöver du ett offentligt tillgängligt API eller GraphQL-slutpunkt. I videon förklaras även hur du skapar en enkel `mesh.json` som ska användas med din Commerce-instans. Mer information och kodexempel finns på [Skapa ett nät](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## Vem är den här videon till?

* Alla nybörjare i API-nät
* Utvecklare som är intresserade av att kombinera flera GraphQL- och API-källor
* Alla som behöver veta hur man filtrerar nätverksfliken och filtrerar efter GraphQL

## Videoinnehåll

* Använda API-nät som omvänd proxy
* Skapa ett nät från en JSON-konfigurationsfil
* Åtkomst till den nyskapade GraphQL-slutpunkten

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Skapa JSON-konfigurationsfilen

API Mesh använder en JSON-konfigurationsfil för att definiera dina källhanterare. JSON-filen innehåller en `sources` -array som innehåller källor för ditt nät. Här är ett exempel på ett nät med en enda källa.

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
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
