---
title: Skapa en paketprodukt
description: Lär dig hur du skapar en paketprodukt med REST API och Commerce Admin.
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# Skapa en paketprodukt

En paketprodukt är ett sätt att gruppera flera produkter under en överordnad produkt. Dessa underordnade produkter kan vara en definierad uppsättning produkter eller erbjuda några varianter som erbjuder flexibla konfigurationsalternativ för kunder. Det tar lite längre tid att konfigurera paketprodukttyper, och du måste göra lite planering innan du konfigurerar dem. Men att erbjuda paketprodukter förbättrar köpupplevelsen genom att göra det enklare för kunderna att anpassa sina produktval.

Du kan till exempel erbjuda ett produktpaket som heter `Learning to surf` i din webbutik. Paketet är den överordnade produkten som fungerar som behållare för de tilldelade underordnade produkterna och anger tillgängliga alternativ:

- Ett standardsurfbord
- Ett vanligt surfbords-utslag
- Rödsurfbrädor

Om du vill ha ytterligare flexibilitet rekommenderas du att tillåta flera alternativ för underordnade produkter. Detta kräver en mer komplex användning av alternativ och underordnade produkter. Om du vill utöka det föregående exemplet är de sista alternativen:

- Ett standardsurfbord
- Ett vanligt surfbords-utslag
- Val av finfärg:
   - Röd
   - Blå
   - Gul

Oavsett om paketet är en statisk grupp av enkla produkter eller flera produkter med variationer gör de flexibla konfigurationsalternativen att paketera produkttyper är ett unikt och kraftfullt marknadsföringsverktyg för Adobe Commerce Store.

Innan du skapar en paketprodukt bör du kontrollera att alla enkla produkter som ska ingå i paketprodukten finns i Adobe Commerce. Skapa alla som inte finns.

I den här självstudiekursen får du lära dig hur du skapar en paketprodukt med REST API och Adobe Commerce Admin.

Använd REST API för att definiera en paketprodukt:

1. Skapa enkla produkter som kan användas i paketprodukten.
1. Skapa en paketprodukt och associera de enkla produkterna.
1. Skaffa paketprodukten och alla tillhörande alternativ.
1. Ta bort ett alternativ från en del av paketprodukterna.
1. Återställ alternativen för en paketprodukt.

När du skapar paketprodukter från Adobe Commerce Admin kan du antingen skapa de enkla produkterna först eller använda det automatiska verktyget för att skapa enkla produkter med hjälp av en guide.

## Vem är den här videon till?

- Webbplatschefer
- e-handlare
- Nya Adobe Commerce-utvecklare som vill lära sig skapa paketprodukter i Adobe Commerce med REST API

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## Skapa produkter med REST

Följande kommandon skapar alla produkter som behövs för att definiera paketprodukten i det här exemplet: fem enkla produkter och en paketprodukt med tre alternativ.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute-set": 4` så att `4` ersätts med attributuppsättnings-ID:t från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## Skapa en paketprodukt och tilldela de enkla produkterna som alternativ

Skapa en paketprodukt genom att skicka följande POST.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute_set_id": 4,` och ersätt `4` med attributuppsättnings-ID:t från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## Ta bort ett alternativ från en paketprodukt

Ta bort en underordnad produkt från en paketprodukt utan att ta bort produkten från katalogen genom att skicka följande DELETE-begäran med cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## Återställ produktalternativ

När du uppdaterar alternativ för paketprodukter måste du ta med alla alternativ som du vill associera med den här produkten. Om den ursprungliga uppsättningen med alternativ innehöll tre produkter och en togs bort, tar du med alla tre alternativen i produktpaketet för att vara säker på att produktpaketet anger alla alternativ. Om du bara tog med det alternativ du tog bort innehåller det uppdaterade produktpaketet bara det alternativet.

Leta reda på alternativ-ID genom att granska svaret från det att paketet skapades för paketprodukten. I det svaret är `option_id` `35`.

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

Uppdatera produktpaketet för att lägga till det alternativ du tog bort genom att skicka följande begäran om POST.

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## Ytterligare resurser

- [Skapa en produktsjälvstudiekurs](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [Paketprodukt](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html){target="_blank"}
- [Adobe Developer REST-självstudiekurser](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
