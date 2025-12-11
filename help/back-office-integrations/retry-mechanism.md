---
title: Använd den inbyggda funktionen för en återförsöksmekanism
description: Utnyttja Adobe I/O Events återförsöksmekanism för flexibla applikationer, inklusive återförsöksvillkor och visuella indikatorer.
landing-page-description: Förstå och utnyttja Adobe I/O Events inbyggda mekanism för återförsök för att förbättra applikationens motståndskraft och hantera händelseaktiveringar effektivt.
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# Utnyttja Adobe I/O Events återförsöksmekanism för programflexibilitet

I videon finns en omfattande guide om hur man utnyttjar Adobe I/O Events inbyggda mekanism för återförsök för att förbättra applikationens motståndskraft. Lär dig hur specifika HTTP-svarets statuskoder utlöser nya händelseförsök. Adobe I/O Events har exponentiella och fasta strategier för återförsök, med intervaller som ökar från en minut till 15 minuter. Dokumentationen innehåller även information om hur återförsöksindikatorer visas i utvecklarkonsolen, med visuella indikeringar som varningsikoner och cirkulära pilar som anger misslyckade respektive nyligen provade händelser.

Lär dig hur återförsöksfunktionen fungerar i kontexten för&quot;konsumentåtgärder&quot; och avgöra om en händelse provas igen. Slutförda svar anges med en 200-statuskod, medan felsvar innehåller ett felobjekt med attributet &#39;statusCode&#39;. Miljöåtgärden&quot;Consumer&quot; avgör HTTP-svarskoden som returneras baserat på resultat från efterföljande bearbetning, vilket säkerställer effektiv händelsehantering och slutligen lyckade aktiveringar.

## Målgrupp

* Utvecklare som vill förstå de specifika HTTP-svarsstatuskoderna som utlöser händelseåterförsök.
* Team som vill lära sig mer om de exponentiella och fasta strategier som Adobe I/O Events använder för återförsök.
* Utvecklare som vill förstå hur visuella indikatorer i utvecklarkonsolen representerar misslyckade händelser och försökte utföra händelser igen.

## Videodonent

* Adobe I/O Events har en inbyggd mekanism för att försöka igen som automatiskt återställer händelseaktiveringar baserat på specifika HTTP-svarskoder.
* Den återförsöksmekanism som Adobe I/O Events implementerar innefattar exponentiella och fasta strategier för säkerhetskopiering.
* Visuella indikatorer i utvecklarkonsolen, till exempel varningsikoner för misslyckade händelser och cirkelformade pilikoner för nyligen utförda händelser.
* Kundens körningsåtgärder spelar en viktig roll när det gäller att fastställa lämpliga HTTP-svarskoder för händelsehantering.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Relaterad dokumentation

* [Webkrok kan inte hantera en händelse](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webkrok nere och markerad som instabil](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
