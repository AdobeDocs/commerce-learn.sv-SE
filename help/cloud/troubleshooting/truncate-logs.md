---
title: Trunkera loggar
description: Lär dig trimma en misslyckad distribution på grund av en full hårddisk genom att trunkera stora loggfiler.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
exl-id: 4a36de40-fb55-41ad-afef-35fc18a271ec
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# Trunkera loggar

Lär dig hur du trimmar och misslyckade distributioner på grund av en full hårddisk. Lär dig hur du hittar och vilka kommandon som kan köras för att frigöra utrymme i Adobe Commerce Cloud-miljön.

Om du tror att du behöver de här loggfilerna kan du `rsync` dem eller använda andra metoder för att få en kopia tillgänglig från servern innan du trunkerar dem.

## Vem är den här videon för

- Utvecklare och IT-personal
- Systemadministratörer

## Videoinnehåll

- Diagnostisera och lösa en misslyckad distribution
- Där några vanliga stora loggfiler hittas
- Snabbmetod för att korta av en loggfil

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## Kommandon som används i videon

För att kontrollera hårddiskutrymme `df -h`. Var uppmärksam på raden dev/mapper/xxxx

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


Visa filerna och deras storlekar i läsbart format som kb, mb och gb med kommandot `ls -lah`

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## Exempel på trunkeringslogg

När du har sparat till rätt projekt och miljö ändrar du till katalogen `var/log`. Sedan kan du trunkera en fil med något som liknar `> some-log-file.log`

```BASH
> support_report.log 
> cron.log 
```

## Relaterad dokumentation

- [Hälsoaviseringar](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
