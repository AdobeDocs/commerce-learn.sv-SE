---
title: Konfigurera Adobe Commerce
description: Lär dig hur du konfigurerar Adobe Commerce så att händelser kan användas i Adobe Developer App Builder.
landing-page-description: Lär dig hur du konfigurerar Adobe Commerce att använda händelsemekanismen för Adobe Developer App Builder.
short-description: Learn how to configure Adobe Commerce to use the event mechanism for consumption by Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---


# Konfigurera Adobe Commerce

Lär dig hur du konfigurerar Adobe Commerce för att visa händelserna. Ytterligare dokumentation finns på [Installera Adobe I/O-händelser för Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare i Adobe Commerce och Adobe Developer App Builder med I/O-händelser och som behöver skapa ett Adobe App Builder-projekt.

## Videoinnehåll {#video-content}

* Konfiguration av Adobe I/O-händelser i Commerce Admin
* Spara en privat nyckel i Commerce-administratören
* Sparar den unika identifieraren i Commerce-administratören
* Skapa en händelseprovider

>[!VIDEO](https://video.tv.adobe.com/v/3415799)

## Användbara kommandon {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
