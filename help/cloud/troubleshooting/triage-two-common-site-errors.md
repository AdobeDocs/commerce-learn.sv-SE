---
title: Diagnostisera och åtgärda några vanliga Commerce Cloud-fel
description: Lös två vanliga Adobe Cloud-projektfel som förhindrar att webbplatsen läses in.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Tjänsten Diagnostic and fix är inte tillgänglig och ett fel uppstod

Lär dig hur du trimmar och löser två vanliga fel som kan uppstå i Adobe Commerce Cloud-projekt.  Lär dig hur och varför dessa fel inträffar och vilka åtgärder som rekommenderas för att åtgärda dem.

## Vem är den här videon för

- Utvecklare och IT-personal
- Systemadministratörer

## Videoinnehåll

- Diagnostisera och åtgärda lagringsproblem:
- Hantera underhållsläge
- Effektiva felsökningstips

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## Kommandon som används i videon

Hitta de fem sista raderna i den undantagslogg som omnämns i svarsmeddelandet.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

För att kontrollera hårddiskutrymme. Var uppmärksam på raden dev/mapper/xxxx

```SHELL
df -h
```

Här hittar du de 15 största filerna

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Visa status för underhållsläge

```SHELL
php bin/magento maintenance:status
```

Inaktivera underhållsläget

```SHELL
php bin/magento maintenance:disable 
```
