---
title: Skapa en modul
description: Skapa en sida som returnerar json med en parameter.
topic: Development
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: fb2cb5b844c4156802e8627e71fc18f8e7fa981d
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Skapa en modul

Modulen är ett strukturelement i [!DNL Commerce] - hela systemet bygger på moduler. Det första steget i att skapa en anpassning är oftast att skapa en modul.

## Vem är den här videon till?

- Utvecklare

## Steg för att lägga till en modul

- Skapa modulmappen.
- Skapa filen etc/module.xml.
- Skapa filen registration.php.
- Kör installationen av bin/magento.
- Uppgradera skript för att installera den nya modulen.
- Kontrollera att modulen fungerar.

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## Användbara resurser

- [Referenshandbok för modul](https://developer.adobe.com/commerce/php/module-reference/)
