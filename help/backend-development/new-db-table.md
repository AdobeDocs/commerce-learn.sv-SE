---
title: Lägga till en tabell i en databas
description: "[!DNL Commerce] har en särskild funktion som gör att du kan skapa databastabeller, ändra befintliga tabeller och till och med lägga till data i dem."
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Lägga till en tabell i en databas

>[!IMPORTANT]
>
>Detta rekommenderas inte längre, se https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] har en särskild funktion som gör att du kan skapa databastabeller, ändra befintliga tabeller och till och med lägga till data i dem, som konfigurationsdata, som måste läggas till när en modul installeras. Denna mekanism gör det möjligt att överföra dessa ändringar mellan olika installationer.

I stället för att utföra manuella SQL-åtgärder upprepade gånger när systemet installeras om skapar utvecklare ett installations- (eller uppgraderingsskript som innehåller data. Skriptet körs varje gång en modul installeras.

## Vem är den här videon till?

- Utvecklare

## Videoinnehåll

- Skapa en modul
- Skapa InstallSchema-skript
- Skapa InstallDataScript
- Lägg till en ny modul och verifiera att en tabell med data har skapats
- Skapa ett UpgradeSchema-skript
- Skapa ett UpgradeData-skript
- Kör uppgraderingsskript och verifiera att tabellen har ändrats

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Användbara resurser

- [Migrera installations-/uppgraderingsskript till deklarativt schema](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
