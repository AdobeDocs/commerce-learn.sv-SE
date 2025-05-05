---
title: Skapa en konfigurerbar produkt
description: Lär dig hur du skapar en konfigurerbar produkt med REST API och Commerce Admin.
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---

# Skapa en konfigurerbar produkt

En konfigurerbar produkt är en överordnad produkt av flera enkla produkter. Definiera en konfigurerbar produkt för att kräva att köparen gör ett eller flera val för att välja en specifik produktvariation. Om produkten till exempel är en skjorta måste köparen välja storlek och färg för att kunna välja skjortan.

Även om en konfigurerbar produkt använder fler SKU:er och till att börja med tar lite längre tid att konfigurera, kan det spara tid i slutänden. Om du planerar att utöka din verksamhet är den konfigurerbara produkttypen ett bra val för produkter med flera alternativ.

Innan du skapar en konfigurerbar produkt bör du kontrollera att alla enkla produkter som ska ingå i den konfigurerbara produkten är tillgängliga i Adobe Commerce. Skapa alla som inte finns.

I den här självstudiekursen får du lära dig hur du skapar en konfigurerbar produkt med REST API och Adobe Commerce Admin.

Använd REST API för att skapa en konfigurerbar produkt:

1. Hämta attributen för en [attributuppsättning](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=sv-SE) för att använda ID-nummer för efterföljande API-anrop.
1. Skapa enkla produkter som kan användas i den konfigurerbara produkten.
1. Skapa en tom konfigurerbar produkt och associera de enkla produkterna.
1. Ange produktattributen för den konfigurerbara produkten.
1. Fyll i den tomma konfigurerbara produkten med enkla produkter.
1. Hämta den konfigurerbara produkten och alla attribut.
1. Hämta tilldelade underordnade produkter för den konfigurerbara produkten.
1. Ta bort kopplingen mellan enkla produkter och konfigurerbara produkter.

När du skapar konfigurerbara produkter från Adobe Commerce Admin kan du antingen skapa de enkla produkterna först eller använda det automatiska verktyget som skapar nya enkla produkter som kan användas med guiden.

## Vem är den här videon till?

- Webbplatschefer
- e-handlare
- Nya Adobe Commerce-utvecklare som vill lära sig hur man skapar konfigurerbara produkter i Adobe Commerce med REST API

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3455039?learn=on&captions=swe)

## Hämta färgattributen med cURL

I det här exemplet returneras hela attributuppsättningen med alla enskilda attribut för attributuppsättningen 10. Det kan vara långt, hundratals linjer är inte ovanliga. När du granskar svaret kommer attribut-ID:t för färg troligen att finnas i mitten. Gör sökningen efter dessa värden snabbare genom att använda grep eller andra metoder för att söka efter resultaten. Mitt svar var nära linje 665 och ingår i följande utdrag från JSON-svaret.

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


Om du vill hämta attribut-ID:n för att konfigurera din konfigurerbara produkt uppdaterar du `attribute-sets/10/attributes`-delen av följande cURL-begäran så att `10` ersätts med attributuppsättnings-ID:t i din miljö. Den här begäran använder metoden GET.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Skapa den första enkla produkten med cURL

### Justera miljö-ID:n och produktinformation

Skapa den första enkla produkten med API:t för att skicka följande POST-förfrågan med cURL.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute-set": 10` så att `10` ersätts med attributuppsättnings-ID:t från din miljö.
- Ändra `"value": "13"` så att `13` ersätts med värdet från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## Skapa den andra enkla produkten med cURL

Skapa den andra enkla produkten med API:t för att skicka följande POST-förfrågan med cURL.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute_set_id": 10,` och ersätt `10` med attributuppsättnings-ID:t från i din miljö.
- Ändra `"value": "14"` och ersätt `14` med värdet från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## Skapa den tredje enkla produkten med cURL

Skapa den tredje enkla produkten genom att skicka följande POST med cURL.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute_set_id": 10,` så att `10` ersätts med attributuppsättnings-ID:t från din miljö.
- Ändra `"value": "15"` och ersätt `15` med värdet från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## Skapa en tom konfigurerbar produkt med cURL

Skapa en tom konfigurerbar POST genom att skicka följande begäran med cURL.

Innan du skickar begäran ska du uppdatera exemplet med värden för miljön.

- Ändra `"attribute_set_id": 10,` och ersätt `10` med attributuppsättnings-ID:t från din miljö.
- Ändra `"value": "93"` och ersätt `93` med värdet från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## Ange tillgängliga alternativ för den konfigurerbara produkten

Ange tillgängliga alternativ för den konfigurerbara produkten genom att skicka följande POST med cURL.

Innan du skickar begäran ändrar du `"attribute_id": 93,` så att `93` ersätts med attribut-ID:t från din miljö.

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

Om du glömmer att ange alternativ för den konfigurerbara produkten (överordnad) visas ett fel när du försöker associera en underordnad produkt till den konfigurerbara produkten. Felmeddelandet liknar följande exempel:

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## Länka den underordnade produkten till den konfigurerbara

Nu har du skapat tre enkla produkter:

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

Lägg till dessa enkla produkter som underordnade till den konfigurerbara produkten genom att skicka följande begäran om POST. Skicka en separat begäran för varje produkt.

Uppdatera värdet `childSKU` för varje begäran med värdet för den underordnade produkten som du lägger till. I följande exempel tilldelas den enkla produkten `kids-Hawaiian-Ukulele-red` till den konfigurerbara produkten med SKU:n `Kids-Hawaiian-Ukulele-red`.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## Hämta en konfigurerbar produkt med cURL

Nu när du har skapat en konfigurerbar produkt med tre tilldelade underordnade SKU. Du kan se de länkade ID:na för de tilldelade produkterna genom att skicka följande GET-begäran med cURL. Denna förfrågan returnerar detaljerad information om den konfigurerbara produkten.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

I följande används metoden GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Hämta den underordnade produkten som är kopplad till en konfigurerbar produkt

Returnera endast de underordnade som är associerade med den konfigurerbara produkten genom att skicka följande begäran om GET. Svaret innehåller alla attribut för den underordnade produkten, inklusive SKU och pris.

I följande används metoden GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Ta bort en underordnad produkt från den överordnade konfigurerbara produkten

Du kan ta bort en underordnad produkt från en konfigurerbar produkt utan att ta bort produkten från katalogen genom att skicka följande DELETE-begäran med cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Ytterligare resurser

- [Skapa en konfigurerbar produktsjälvstudiekurs](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [Konfigurerbar produkt](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=sv-SE){target="_blank"}
- [Adobe Developer REST-självstudiekurser](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
