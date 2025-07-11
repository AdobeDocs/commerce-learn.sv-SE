---
title: Lär dig hur du skapar en modul i Adobe Commerce för att använda händelser.
description: Lär dig hur du skapar en Commerce-modul för att använda händelser.
landing-page-description: Lär dig hur du skapar en Adobe Commerce-modul för att använda händelser.
short-description: Lär dig hur du skapar en Adobe Commerce-modul för att använda händelser.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Utveckling av modulen Adobe Commerce

Lär dig hur du registrerar händelser, hittar händelser som stöds och hur du använder en ny XML-fil `io_events.xml` i utvecklingen av anpassade moduler. I videon visas även hur utvecklare kan hitta registrerade händelser som kan användas och hur de kan avbryta prenumerationen på händelser som redan har definierats. Ytterligare dokumentation finns på [Installera Adobe I/O-händelser för Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare på Adobe Commerce och Adobe Developer App Builder med I/O-händelser.

## Videoinnehåll {#video-content}

* Registrera händelser i Commerce för användning i Adobe Developer App Builder
* Identifiera händelser som kan registreras
* Lär dig att registrera händelser i io_events.xml
* Lär dig hur du registrerar händelser i Commerce-instanser `app/etc/config.php`
* Lär dig hur du avbryter prenumerationen på ett evenemang

>[!VIDEO](https://video.tv.adobe.com/v/3430654?quality=12&learn=on&captions=swe)

## Användbara kommandon {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
