---
title: Återställ Admin-URI med CLI
description: Lär dig hur du återställer admin-URI i Adobe Commerce Cloud CLI. Den här metoden är användbar när ändringar i administratörens URL orsakar åtkomstproblem.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# Återställ admin-URI:n med klippet

Lär dig hur du återställer admin-URI med kommandot Adobe Commerce Cloud Cli. Detta är användbart om admin-URL:en har ändrats från admin men ett fel har gjorts och du inte längre kan komma åt admin.

>[!VIDEO](https://video.tv.adobe.com/v/3439696?captions=swe&learn=on)

## Vissa kommandon som används i självstudien

Ändra konfigurationen så att en anpassad administratörssökväg (URL) förväntas till 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Ändra konfigurationen för den anpassade administratörssökvägen till 0

`$ php bin/magento config:set admin/url/use_custom_path 0`
