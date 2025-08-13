---
title: Hur man hämtar data
description: Lär dig hur du hämtar data för en Adobe Commerce Optimizer-instans.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Fråga Adobe Commerce Optimizer

Lär dig hur du hämtar data med GraphQL i en Adobe Commerce Optimizer-instans.

## Vem är den här videon till?

* Commerce Solution Architect och utvecklare

## Videoinnehåll

* Fråga data med GraphQL
* Använda jq för att göra JSON enklare att läsa

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## Kodexempel

Se till att utbyta värden som `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-source-locale}}` och `{{your-search-query-string}}` med de värden som behövs i din fråga.

Enkel exempelfråga

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Grundläggande provfråga med `jq` för att göra en vacker utskrift

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Relaterat innehåll

* [Komma igång med Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] Guide](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
