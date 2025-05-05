---
title: Skapa en presentkortsprodukt
description: Lär dig hur du skapar en presentkortsprodukt med REST API och Commerce Admin.
kt: 14587
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-2-1
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
duration: 579
exl-id: c18fd80e-1a25-4346-a8c5-3b5449d49965
source-git-commit: 48a98261a827741459e45f14f7463f4a989c49d2
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Skapa en presentkortsprodukt

Lär dig hur du skapar en presentkortsprodukt med REST API och Adobe Commerce Admin.

## Vem är den här videon till?

- Webbplatschefer
- e-handlare
- Nya Adobe Commerce-utvecklare som vill lära sig hur man skapar produkter i Adobe Commerce med REST API.

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3453081?learn=on&captions=swe)

## Skapa ett presentkort med en enkel nyttolast

I följande exempel visas nyttolasten för att skapa ett presentkort som det som visas i videon. Den här mindre nyttolasten åsidosätter standardinställningarna för en delmängd av de tillgängliga attributen. De återstående attributen som inte ingår i nyttolasten förblir inställda på standardvärden.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=83b55460e3ab1bf903fab59dfedc81c6; private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "giftcard21",
    "name": "giftcard21",
    "attribute_set_id": {{Your Attribute Set ID}},
    "price": 0,
    "status": 1,
    "visibility": 4,
    "type_id": "giftcard",
    "weight": 1,
    "extension_attributes": {
      "website_ids": [
        1
      ],
      "stock_item": {
        "qty": 1000,
        "is_in_stock": true
      },
      "giftcard_amounts": [
        {
          "attribute_id": {{Your attribute ID for giftcard_amounts},
          "website_id": 0,
          "value": 10,
          "website_value": null
        },
        {
          "attribute_id": {{Your attribute ID for giftcard_amounts},
          "website_id": 0,
          "value": 20,
          "website_value": null
        }
      ]
    },
    "custom_attributes": [
    
      {
        "attribute_code": "gift_message_available",
        "value": "2"
      },
      {
        "attribute_code": "is_redeemable",
        "value": "1"
      },
      {
        "attribute_code": "required_options",
        "value": "1"
      },
      {
        "attribute_code": "use_config_is_redeemable",
        "value": "1"
      },
      {
        "attribute_code": "has_options",
        "value": "1"
      },
      {
        "attribute_code": "allow_message",
        "value": "1"
      },
      {
        "attribute_code": "giftcard_type",
        "value": "1"
      },
      {
        "attribute_code": "giftcard_amounts",
        "value": [
          {
            "website_id": "0",
            "value": "10.0000",
            "attribute_id": "{{Your attribute ID for giftcard_amounts}",
            "website_value": 10
          },
          {
            "website_id": "0",
            "value": "20.0000",
            "attribute_id": "{{Your attribute ID for giftcard_amounts}",
            "website_value": 20
          }
        ]
      },
      {
        "attribute_code": "allow_open_amount",
        "value": "1"
      },
      {
        "attribute_code": "open_amount_min",
        "value": "10.000000"
      },
      {
        "attribute_code": "open_amount_max",
        "value": "100.000000"
      },
      {
        "attribute_code": "is_returnable",
        "value": "2"
      }
    ]
  }
}'
```

## Skapa ett presentkort med en fullständig nyttolast

I följande exempel visas POSTENS begäran om att skapa ett presentkort med en full nyttolast. Nyttolasten innehåller alla attribut som kan konfigureras när du skapar ett presentkort. Om du använder det här kodexemplet anpassar du konfigurationen genom att uppdatera standardvärdena för varje attribut efter behov innan du skickar begäran.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=83b55460e3ab1bf903fab59dfedc81c6; private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "giftcard22",
    "name": "giftcard22",
    "attribute_set_id": {{Your Attribute Set ID}},
    "price": 0,
    "status": 1,
    "visibility": 4,
    "type_id": "giftcard",
    "created_at": "2024-01-24 20:54:54",
    "updated_at": "2024-01-24 20:54:54",
    "weight": 1,
    "extension_attributes": {
      "website_ids": [
        1
      ],
      "stock_item": {
        "qty": 1000,
        "is_in_stock": true,
        "is_qty_decimal": false,
        "show_default_notification_message": false,
        "use_config_min_qty": true,
        "min_qty": 0,
        "use_config_min_sale_qty": 1,
        "min_sale_qty": 1,
        "use_config_max_sale_qty": true,
        "max_sale_qty": 10000,
        "use_config_backorders": true,
        "backorders": 0,
        "use_config_notify_stock_qty": true,
        "notify_stock_qty": 1,
        "use_config_qty_increments": true,
        "qty_increments": 0,
        "use_config_enable_qty_inc": true,
        "enable_qty_increments": false,
        "use_config_manage_stock": true,
        "manage_stock": true,
        "low_stock_date": null,
        "is_decimal_divided": false,
        "stock_status_changed_auto": 0
      },
      "giftcard_amounts": [
        {
          "attribute_id": {{Your attribute ID for giftcard_amounts},
          "website_id": 0,
          "value": 10,
          "website_value": null
        },
        {
          "attribute_id": {{Your attribute ID for giftcard_amounts},
          "website_id": 0,
          "value": 20,
          "website_value": null
        }
      ]
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": [],
    "custom_attributes": [
      {
        "attribute_code": "options_container",
        "value": "container2"
      },
      {
        "attribute_code": "url_key",
        "value": "giftcard22"
      },
      {
        "attribute_code": "gift_message_available",
        "value": "2"
      },
      {
        "attribute_code": "is_redeemable",
        "value": "1"
      },
      {
        "attribute_code": "required_options",
        "value": "1"
      },
      {
        "attribute_code": "use_config_is_redeemable",
        "value": "1"
      },
      {
        "attribute_code": "has_options",
        "value": "1"
      },
      {
        "attribute_code": "lifetime",
        "value": "0"
      },
      {
        "attribute_code": "use_config_lifetime",
        "value": "1"
      },
      {
        "attribute_code": "email_template",
        "value": "giftcard_email_template"
      },
      {
        "attribute_code": "use_config_email_template",
        "value": "1"
      },
      {
        "attribute_code": "allow_message",
        "value": "1"
      },
      {
        "attribute_code": "meta_title",
        "value": "giftcard22"
      },
      {
        "attribute_code": "use_config_allow_message",
        "value": "1"
      },
      {
        "attribute_code": "gift_wrapping_available",
        "value": "2"
      },
      {
        "attribute_code": "meta_keyword",
        "value": "giftcard22"
      },
      {
        "attribute_code": "giftcard_type",
        "value": "1"
      },
      {
        "attribute_code": "giftcard_amounts",
        "value": [
          {
            "website_id": "0",
            "value": "10.0000",
            "attribute_id": "{{Your attribute ID for giftcard_amounts}",
            "website_value": 10
          },
          {
            "website_id": "0",
            "value": "20.0000",
            "attribute_id": "{{Your attribute ID for giftcard_amounts}",
            "website_value": 20
          }
        ]
      },
      {
        "attribute_code": "allow_open_amount",
        "value": "1"
      },
      {
        "attribute_code": "open_amount_min",
        "value": "10.000000"
      },
      {
        "attribute_code": "open_amount_max",
        "value": "100.000000"
      },
      {
        "attribute_code": "meta_description",
        "value": "giftcard22"
      },
      {
        "attribute_code": "is_returnable",
        "value": "2"
      }
    ]
  }
}'
```

## Ytterligare resurser

- [Skapa en presentkortsprodukt från Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-gift-card-create.html?lang=sv-SE){target="_blank"}
- [Adobe Developer REST-självstudiekurser](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
