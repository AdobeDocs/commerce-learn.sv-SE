---
title: Skapa en nedladdningsbar produkt
description: Lär dig hur du skapar en nedladdningsbar produkt med REST API och Adobe Commerce Admin.
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Skapa en nedladdningsbar produkt

Lär dig hur du skapar en nedladdningsbar produkt med REST API och Adobe Commerce Admin.

## Vem är den här videon till?

- Webbplatschefer
- e-handlare
- Nya Adobe Commerce-utvecklare som vill lära sig hur man skapar produkter i Adobe Commerce med REST API

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3453947?learn=on&captions=swe)

## Tillåtna nedladdningsbara domäner

Du måste ange vilka domäner som tillåts för nedladdning. Domäner läggs till i projektets `env.php`-fil via kommandoraden. Filen `env.php` innehåller information om de domäner som tillåts innehålla hämtningsbart innehåll. Ett fel inträffar om en nedladdningsbar produkt skapas med REST API _innan_ filen `php.env` uppdateras:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

Anslut till servern för att ange domänen: `bin/magento downloadable:domains:add www.example.com`

När det är klart ändras `env.php` inuti arrayen _downloadable_domains_.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Nu när domänen har lagts till i `env.php` kan du skapa en nedladdningsbar produkt i Adobe Commerce Admin eller genom att använda REST API.

Mer information finns i [Konfigurationsreferens](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=sv-SE#downloadable_domains).

>[!IMPORTANT]
>I vissa versioner av Adobe Commerce kan följande fel uppstå när en produkt redigeras i Adobe Commerce Admin. Produkten skapas med REST API, men den länkade nedladdningen har ett pris på `null`.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

Använd API:t för uppdateringslänk om du vill åtgärda felet: `POST V1/products/{sku}/downloadable-links.`

Mer information finns i avsnittet [Uppdatera en produktnedladdningslänk med cURL](#update-downloadable-links).

## Skapa en nedladdningsbar produkt med cURL (hämta från fjärrserver)

I det här exemplet visas hur du skapar en nedladdningsbar produkt med cURL när filen som ska laddas ned inte finns på samma server. Detta är vanligt om filen lagras i en S3-bucket eller någon annan digital resurshanterare.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## Skapa en nedladdningsbar produkt med cURL (ladda ned från Commerce programserver)

I det här exemplet visas hur du använder cURL för att skapa en nedladdningsbar produkt från Adobe Commerce Admin när filen lagras på samma server som Adobe Commerce-programmet.

I det här fallet överförs filen till katalogen `pub/media/downloadable/files/links/` när administratören som hanterar katalogen väljer `upload file`.  Automatisering skapar och flyttar filerna till respektive plats baserat på följande mönster:

- Varje överförd fil lagras i en mapp baserat på de två första tecknen i filnamnet.
- När överföringen initieras skapar eller använder Commerce-programmet befintliga mappar för att överföra filen.
- När du hämtar filen används den del av sökvägen som är kopplad till katalogen `pub/media/downloadable/files/links/` i `link_file`-avsnittet av sökvägen.

Om den överförda filen till exempel har namnet `download-example.zip`:

- Filen överförs till sökvägen `pub/media/downloadable/files/links/d/o/`.
Underkatalogerna `/d` och `/d/o` skapas om de inte finns.

- Den sista sökvägen till filen är `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- Värdet `link_url` för det här exemplet är `d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## Skaffa en produkt med hjälp av en licens

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Uppdatera produkten med Postman {#update-downloadable-links}

Använd slutpunkten `rest/all/V1/products/{sku}/downloadable-links`
`SKU` är det produkt-ID som genererades när produkten skapades. I kodexemplet nedan är det till exempel siffran 39, men se till att den uppdateras för att använda ID:t från din webbplats. Länkarna för de hämtningsbara produkterna uppdateras.

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## Uppdatera en nedladdningslänk med CURL

När du uppdaterar en produktnedladdningslänk med cURL, innehåller URL:en SKU:n för den produkt som uppdateras.  I följande kodexempel är SKU `abcd12345`. När du skickar kommandot ändrar du värdet så att det matchar SKU:n för den produkt du vill uppdatera.

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## Ytterligare resurser

- [Hämtningsbar produkttyp](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=sv-SE){target="_blank"}
- [Konfigurationshandbok för hämtningsbara domäner](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=sv-SE#downloadable_domains){target="_blank"}
- [Adobe Developer REST-självstudiekurser](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
