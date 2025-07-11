---
title: Lär dig installera IO-händelser för Adobe Commerce 2.4.5
description: Lär dig hur du installerar moduler som behövs för IO-händelser i Adobe Commerce 2.4.5 för användning i Adobe Developer App Builder
landing-page-description: Lär dig hur du installerar flera moduler som behövs för Adobe Commerce 2.4.5 med Composer.
short-description: Lär dig hur du installerar flera moduler som behövs för Adobe Commerce 2.4.5 med Composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Installation av Adobe Commerce 2.4.5

Lär dig installera flera nya moduler i Adobe Commerce med Composer för version 2.4.5. Här anges vilka moduler som ska användas i Adobe Commerce-programmet. Ytterligare dokumentation finns på [Installera Adobe I/O-händelser för Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare på Adobe Commerce och Adobe Developer App Builder med I/O Events

## Videoinnehåll {#video-content}

* Installation av nödvändiga moduler med Composer
* Kommandon som ska köras för lokal värdtjänst
* Kommandon som ska köras för Adobe Commerce Cloud
* Redigering krävs för Adobe Commerce Cloud-ljud

>[!VIDEO](https://video.tv.adobe.com/v/3430647?quality=12&learn=on&captions=swe)

## Användbara kommandon {#useful-commands}

Det finns olika kommandon som skiljer sig något åt, beroende på om du använder en egen värdmiljö eller Adobe Commerce Cloud.

### On Premise-värdtjänster {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce i molnet {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
