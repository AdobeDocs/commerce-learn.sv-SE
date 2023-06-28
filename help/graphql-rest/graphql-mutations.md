---
title: Utför en mutation med GraphQL
description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Utför din första mutation med POSTER.
landing-page-description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Utför din första mutation med POSTER.
short-description: Få en introduktion om att utföra en mutation med GraphQL på Adobe Commerce och [!DNL Magento Open Source]. Utför din första mutation med POSTER.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Mutationer

Alla fullständiga API-specifikationer måste kunna erbjuda både möjlighet att fråga efter data och att skapa och uppdatera dem.

REST skiljer mellan begäranden som ändrar data och sådana som inte har begärandetypen eller verb (GET kontra POST eller PUT).
När du använder GraphQL särskiljs dataändringsfrågor av `mutation` nyckelord som motsvarar en annan rottyp i schemat som definierats på servern.

Titta på den här exempelmutationen för att lägga en produkt i en användares kundvagn. (Detta kräver ett kundvagn-ID som genererades för den inloggade kundens session eller med `createEmptyCart` mutation.)

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

Det viktigaste att notera är att med det ovanstående exemplet är det, förutom att `mutation` nyckelord i stället för `query`är syntaxen identisk med en fråga. På samma sätt som i frågor innehåller mutationen:

* Ett godtyckligt åtgärdsnamn (`doAddToCart`)
* En lista med variabler (till exempel `$cartId`)
* Ett inledande fält (`addProductsToCart`) med argument (till exempel `cartId`, ställs in på värdet för `$cartId`) inom parentes
* En delmarkering av fält inom klammerparenteser

Med fältdelmarkeringen kan du på ett flexibelt sätt definiera de fält som du vill returnera (från den typ som tilldelats som returvärde för `addProductsToCart` - `AddProductsToCartOutput`) när mutationen är klar.

Som tidigare nämnts startar fält som definieras i ett GraphQL-schema på en rottyp för frågor (kallas vanligtvis `Query`). På samma sätt finns det en annan rottyp för mutationer (kallas vanligtvis `Mutation`). `addProductsToCart` är ett fält i den rottypen.

Några andra kommentarer om exemplet ovan:

* The `!` teckensuffix `String` och `CartItemInput` anger att variabeln är obligatorisk.
* Hakparenteser (`[]`) runt `CartItemInput` typ angiven för `$cartItems` anger en lista av den typen i stället för ett enda värde.

{{$include /help/_includes/graphql-rest-related-links.md}}
