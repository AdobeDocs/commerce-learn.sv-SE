---
title: Lär dig hur du använder villkorliga händelser i Adobe Commerce
description: Lär dig hur du använder villkorliga händelser som ska användas i Adobe Developer App Builder.
landing-page-description: Lär dig hur du använder villkorliga Adobe Commerce-händelser.
short-description: Lär dig hur du använder villkorliga Adobe Commerce-händelser.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Adobe Commerce villkorsstyrda evenemang

Läs om villkorliga händelser i Adobe Commerce som kan användas i Adobe Developer App Builder. Ytterligare dokumentation finns på [Installera Adobe I/O Events för Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare på Adobe Commerce och Adobe Developer App Builder med I/O-event och som behöver skapa ett Adobe App Builder-projekt.

## Videoinnehåll {#video-content}

* Läs om villkorliga händelser
* Lär dig hur du använder den nya XML-filen io_events.xml
* Lär dig hur du konfigurerar villkorliga händelser
* Definiera regler för användning i villkorshändelser
* Lär dig hur du registrerar händelser i Commerce-instanser `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3430655?captions=swe&quality=12&learn=on)

## Användbara kommandon {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
