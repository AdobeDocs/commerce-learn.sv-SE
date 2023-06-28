---
title: Lär dig hur du installerar IO-händelser för Adobe Commerce 2.4.6
description: Lär dig hur du installerar moduler som behövs för IO-händelser i Adobe Commerce 2.4.6 för användning i Adobe Developer App Builder
landing-page-description: Lär dig hur du installerar flera moduler som behövs för Adobe Commerce 2.4.6.
short-description: Lär dig hur du installerar flera moduler som behövs för Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Installation av Adobe Commerce 2.4.6

Lär dig hur du installerar flera nya moduler i Adobe Commerce med Composer för version 2.4.6. Ytterligare dokumentation finns på [Installera Adobe I/O-händelser för Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Vem är den här videon till?

* Utvecklare som är nybörjare i Adobe Commerce och Adobe Developer App Builder med I/O Events.

## Videoinnehåll {#video-content}

* Kommandon som ska köras för lokal värdtjänst
* Kommandon som ska köras för Adobe Commerce Cloud
* Redigering krävs för Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## Användbara kommandon {#useful-commands}

Det finns olika kommandon som skiljer sig något åt, beroende på om du använder en egen värdmiljö eller Adobe Commerce Cloud.

### On Premise-värdtjänster {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce i molnet {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
