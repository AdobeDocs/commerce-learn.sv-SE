---
title: Skapa en GraphQL-begäran med en enda källa som ska användas i API-nät
description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar en begäran som har en källa.
landing-page-description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar en begäran som har en källa.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Skapa GraphQL API-nät med en källa

Videon hjälper en utvecklare att förstå hur man skapar en omvänd GraphQL-proxy och har en enda källa. Kom ihåg att för att GraphQL Mesh ska fungera som förväntat krävs en offentlig URL med ett giltigt GraphQL-schema. I videon förklaras också hur du konfigurerar din första JSON för användning med e-handelswebbplatsen. Grundläggande kodexempel som användes i videon finns på [Skapa ett nät](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## Vem är den här videon till?

* Alla som är nybörjare i API-nät
* Utvecklare som är intresserade av att använda flera olika grafikkällor
* Alla som behöver veta hur man filtrerar nätverksfliken och filtrerar efter grafer

## Videoinnehåll

* Använda API som omvänd proxy
* JSON-konfiguration med Adobe Developer kommandoradsgränssnitt
* Åtkomst till den nyskapade GraphQL-slutpunkten

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Skapa JSON-konfigurationsfilen

Om du vill att Adobe App Builder ska känna till alla dina källor definierar du dem i en JSON-konfiguration. Varje källa är ett element i en array och du kan ha en eller flera. Här är ett exempel på en enda källa

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
