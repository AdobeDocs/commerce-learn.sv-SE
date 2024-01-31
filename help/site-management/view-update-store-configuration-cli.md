---
title: Lär dig hur du visar och ställer in administratörskonfigurationer med kommandoraden
description: En demonstration om hur du visar och anger konfigureringsvärden med kommandoraden
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: 9d4b01d383e5492ccc0cbd27636a49581e8ffb5b
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# Lär dig hur du visar och ställer in administratörskonfigurationer med kommandoraden

Demo av hur du visar, ställer in och hittar konfigurationsvärden med Commerce CLI. Förstå var värdena sparas och var standardvärdena kommer ifrån.

## Vem är den här videon till?

- Adobe Commerce-utvecklare

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## Vissa kommandon som används i självstudien

Ändra säkerhetsinställningen för lösenord till rekommenderat:

`$ php bin/magento config:set admin/security/password_is_forced 0`

Visa e-postadressen för funktionen för automatisk kopiering av försäljningsorder

`$ php bin/magento config:show sales_email/order/copy_to`

Visa det tomma resultatet för en konfiguration som har ett värde i administratören

`php bin/magento config:show trans_email/ident_sales/email`

## Mysql-frågor som används i självstudien

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## Var hittar du standardsäljens e-postadress

Hur hittar jag konfigurationsvärdet som är definierat någonstans i kodbasen?
`grep -rnw vendor/magento/ -e 'sales@example.com'`

Visa en sida i terminal och visa radnummer `cat -n vendor/magento/module-email/etc/config.xml`

## Ytterligare resurser

- [Kommandoradsverktyg](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [Konfigurera administratörssäkerhet](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
