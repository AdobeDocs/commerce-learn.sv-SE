---
title: Använd Snabbt för att neka åtkomst till en hel webbplats
description: Begränsa åtkomsten till Adobe Commerce Site Access med Edge ACL och en anpassad VCL
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Använd Snabbt för att neka åtkomst till en hel webbplats

Lär dig hur du begränsar åtkomsten till din Adobe Commerce Cloud-webbplats med hjälp av Edge snabbåtkomstkontrollistor och anpassade VCL-fragment. Den här steg-för-steg-guiden hjälper dig att skydda din miljö innan du startar programmet genom att endast tillåta specifika IP-adresser, vilket säkerställer att din utvecklingswebbplats förblir privat.

## Vad du kommer att lära dig

Begränsa åtkomsten till Adobe Commerce-sajten med Edge ACL:er och anpassade VCL | Säkra förstartsmiljöer

## Vem är den här videon till?

* DevOps-tekniker
* Adobe Commerce Developer
* Webbplatstillförlitlighetstekniker

>[!VIDEO](https://video.tv.adobe.com/v/3464783?captions=swe&learn=on)

## Kodexempel

Detta är ett exempel på VCL som används

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Relaterad dokumentation

* [Upptäcker skadlig IP-adress](https://experienceleague.adobe.com/sv/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [Anpassad VCL för att tillåta begäranden](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [Anpassad VCL för blockeringsbegäranden](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)
