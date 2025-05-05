---
title: Skapa en grupperad produkt
description: Lär dig hur du skapar en grupperad produkt med REST API och Commerce Admin.
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 76a67af957b0d8c1eb64ad42f92412f338650d4b
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Skapa en grupperad produkt

En grupperad produkt består av enkla fristående produkter som presenteras som en grupp. Du kan erbjuda varianter av en enskild produkt eller gruppera dem efter årstid eller tema. Innan du skapar en grupperad produkt bör du kontrollera att alla enkla produkter som ska ingå i gruppen är tillgängliga i Adobe Commerce och skapa produkter som inte finns.

I den här självstudien får du lära dig hur du skapar en grupperad produkt med REST API och Adobe Commerce Admin.

Använd REST API för att skapa en gruppprodukt i administratören:

1. Skapa en tom grupperad produkt.
1. Skapa enkla produkter som kan användas i den grupperade produkten.
1. Fyll den tomma grupperade produkten med enkla produkter.
1. Skapa en tom grupperad produkt och associera de enkla produkterna.

   När du associerar enkla produkter med den grupperade produkten används attributet för sorteringsordning (`position`) i nyttolasten av frontend för att visa de associerade produkterna i önskad ordning. Om attributet `position` inte anges visas produkterna i den ordning som de lades till i den grupperade produkten.

Skapa de enkla produkterna först när du skapar grupperade produkter från Adobe Commerce Admin. När du är redo att skapa den grupperade produkten kopplar du de enkla produkterna genom att tilldela dem till den grupperade produkten i en batch.

## Vem är den här videon till?

- Webbplatschefer
- e-handlare
- Nya Adobe Commerce-utvecklare som vill lära sig hur man skapar grupperade produkter i Adobe Commerce med REST API.

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## Inställningar för den grupperade produkten

I det här exemplet finns det tre enkla produkter (skapade först) och en grupperad produkt. Två enkla produkter är kopplade till den grupperade produkten och sedan läggs den tredje enkla produkten till i den grupperade produkten.

## Skapa den första enkla produkten med cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## Skapa den andra enkla produkten med cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## Skapa den tredje enkla produkten med cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## Skapa en tom grupperad produkt med cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## Lägg till den första och den andra enkla produkten i den grupperade produkten

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## Lägg till den tredje enkla produkten i den befintliga grupperade produkten

Inkludera lämpligt positionsnummer (allt utom `1` eller `2`) som används för de två första produkterna som ursprungligen associeras med den grupperade produkten. I detta exempel är positionen `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## Ta bort en enkel produkt från en grupperad produkt

Använd `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}` om du vill [ta bort en enkel produkt](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) från en grupperad produkt.

Om du vill ta reda på vad som ska användas som `{type}` använder du xdebug för att hämta begäran och utvärdera $linkTypes: `related`, `crosssell`, `uupsell` och `associated`.
![Länktyper för grupperad produkt - alt text](/help/assets/site-management/catalog/grouped-types.png "Grupperade produktlänkstyper som hämtats under xdebug-sessionen")

När de enkla produkterna skulle länkas till den grupperade produkten innehöll nyttolasten några avsnitt som liknar:

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

I nyttolasten tillhandahåller värdet `link_type` `associated` det `{type}`-värde som krävs i DELETE-begäran. Begär-URL:en liknar `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

Se cURL-begäran för att ta bort den enkla produkten med SKU:n `product-sku-three` från den grupperade produkten med SKU:n `my-new-grouped-product`:

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## Hämta en grupperad produkt med cURL

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## Ytterligare resurser

- [Skapa och hantera grupperade produkter](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [Grupperad produkt](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=sv-SE){target="_blank"}
- [Adobe Developer REST-självstudiekurser](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
