---
title: Utför en mutation med GraphQL
description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och  [!DNL Magento Open Source]. Utför din första mutation med POST-anrop.
landing-page-description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och  [!DNL Magento Open Source]. Utför din första mutation med POST-anrop.
short-description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och  [!DNL Magento Open Source]. Utför din första mutation med POST-anrop.
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Mutationer

Detta är en del 3 av serien för GraphQL och Adobe Commerce. Mutationer är möjligheten att spara, uppdatera och returnera värden med GraphQL.


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## Relaterade videor och självstudiekurser om GraphQL i den här serien

* [Del 1 GraphQL - Introduktion](../graphql-rest/intro-graphql.md)
* [Del 2 GraphQL - Frågor](../graphql-rest/graphql-queries.md)
* [Del 4 GraphQL - schema](../graphql-rest/graphql-schema.md)

## Exempelmutation

Alla fullständiga API-specifikationer måste kunna erbjuda både möjlighet att fråga efter data och att skapa och uppdatera dem.

REST skiljer mellan förfrågningar som ändrar data och sådana som inte har förfrågningstypen eller verb (GET vs POST eller PUT).
När du använder GraphQL särskiljs datamodifierande frågor med nyckelordet `mutation` som motsvarar ett annat
rottyp i schemat som definierats på servern.

Titta på den här exempelmutationen för att lägga en produkt i en användares kundvagn. (Detta kräver ett vagn-ID som genereras
för den inloggade kundens session eller med mutationen `createEmptyCart`.)

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

Du kan tänka dig att mutationen ovan skickas i en begäran tillsammans med följande variabelordlista:

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

Slutligen kanske du får ett svar som det här:

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

Det viktigaste att notera är att om exemplet ovan används inte nyckelordet `mutation` utan `query`
syntaxen är identisk med en fråga. På samma sätt som i frågor innehåller mutationen:

* Ett godtyckligt åtgärdsnamn (`doAddToCart`)
* En lista med variabler (till exempel `$cartId`)
* Ett inledande fält (`addProductsToCart`) med argument (till exempel `cartId`, inställt på värdet `$cartId`) inom parentes
* En delmarkering av fält inom klammerparenteser

Med fältdelmarkeringen kan du på ett flexibelt sätt definiera de fält som du vill returnera (från den typ som tilldelats som
returvärdet `addProductsToCart` - `AddProductsToCartOutput`) när mutationen är slutförd.

Som förklarats ovan startar fält som definieras i ett GraphQL-schema på en rottyp för frågor (kallas vanligtvis `Query`). På samma sätt
en annan rottyp finns för mutationer (kallas vanligtvis `Mutation`). `addProductsToCart` är ett fält
på den rottypen.

Några andra kommentarer om exemplet ovan:

* Tecknet `!` med suffixet `String` och `CartItemInput` anger att variabeln är obligatorisk.
* Hakparenteserna (`[]`) runt `CartItemInput`-typen som anges för `$cartItems` anger en lista
av den typen i stället för ett enda värde.

{{$include /help/_includes/graphql-rest-related-links.md}}
