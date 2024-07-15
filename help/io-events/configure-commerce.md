---
title: Konfigurera Adobe Commerce
description: Lär dig hur du konfigurerar Adobe Commerce så att händelser kan användas i Adobe Developer App Builder.
landing-page-description: Lär dig hur du konfigurerar Adobe Commerce att använda händelsemekanismen för Adobe Developer App Builder användning.
short-description: Lär dig hur du konfigurerar Adobe Commerce att använda händelsemekanismen för Adobe Developer App Builder användning.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Konfigurera Adobe Commerce

Lär dig hur du konfigurerar Adobe Commerce för att visa händelserna. Ytterligare dokumentation finns på [Installera Adobe I/O-händelser för Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare på Adobe Commerce och Adobe Developer App Builder med I/O-event och behöver skapa ett Adobe App Builder-projekt.

## Videoinnehåll {#video-content}

* Konfiguration av Adobe I/O-händelser i Commerce-administratören
* Spara en privat nyckel i Commerce-administratören
* Spara den unika identifieraren i Commerce-administratören
* Skapa en händelseprovider

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## Användbara kommandon {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
