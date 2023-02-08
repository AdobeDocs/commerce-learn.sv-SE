---
title: Skapa en GraphQL med flera källor som ska användas i API-nät
description: Upptäck hur du kan använda flera källor för API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig mer om några vanliga fel och hur du löser dem.
landing-page-description: Upptäck hur du använder API Mesh på Adobe Commerce och [!DNL Adobe App Builder]. Lär dig hur du skapar en begäran som har flera källor och hur du löser några vanliga fel.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Skapa GraphQL API-nät med flera källor

Videon hjälper en utvecklare att förstå hur man skapar en omvänd GraphQL-proxy med flera olika källor. I den här videon visas hur du sammanfogar olika källor, identifierar fel och sparar Git-ändringar. Grundläggande kodexempel som användes i videon finns på [Skapa ett nät](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## Vem är den här videon till?

* Alla som är nybörjare i API-nät
* Utvecklare som är intresserade av att använda flera olika grafikkällor
* Alla som behöver veta hur man filtrerar nätverksfliken och filtrerar efter grafer

## Videoinnehåll

* Hur komplexa API-scheman för anpassade attribut från en annan källa kan åsidosätta standardkällschemat
* Ändra nätkonfigurationen för API så att den tar hänsyn till det andra åsidosättande schemat
* Felsöka fel som kan uppstå under processen, t.ex. namnkonflikt, schematillgänglighet och annan SDL-syntax
* Exempel på vanliga fel efter försök att fästa scheman
* Återskapa API-nätet efter redigeringar
* Spara ändringar i Git efter att API Mesh-konfigurationen har ändrats

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Skapa JSON-konfigurationsfilen

Om du vill att Adobe App Builder ska känna till alla dina källor definierar du dem i en JSON-konfiguration. Varje källa är ett element i en array och du kan ha en eller flera. Här är ett exempel på en begäran om flera källor som sammanfogas för att bilda ett enda svar.

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
        "name": "ERP",
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
