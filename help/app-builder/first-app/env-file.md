---
title: .env-filen
description: Läs mer om filtyperna i .env-filen för det här exempelprogrammet
landing-page-description: Läs om Adobe Developer App Builder som används med Adobe Commerce och vilka typer av innehåll som används i .env-filen
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Generera och konfigurera .env-filen {#env-file}

`.env` är en specialfil som inte ingår i exempelmodulen, men som är viktig att använda i ditt Adobe Developer App Builder-program. Filen innehåller hemligheter och annan information. Undvik att implementera den här filen i alla koddatabaser.

## Vem är den här videon till?

* Utvecklare som är nybörjare i Adobe Commerce med begränsad erfarenhet av att använda Adobe App Builder och som vill veta mer om filen `.env`.

## Videoinnehåll

* Introduktion till .env-filen och dess syfte
* Generera .env-filen
* Lägga till nya hemligheter i filen
* Undvik att implementera filen eftersom den innehåller känslig information

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

## Kodexempel

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Du kan se dessa statiska värden som används i exempelmodulen i filen `actions/commerce.index.js`.

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
