---
title: Utbyggbarhet utanför processen för Adobe Commerce
description: Lär dig mer om Adobe App Builder och varför det är en viktig aspekt när det gäller utbyggbarhet utanför processen.
landing-page-description: Lär dig vad är App Builder och hur det kan hjälpa dig med Adobe Commerce utvecklingsstrategier.
short-description: Learn what is App Builder and how it can help with Adobe Commerce development strategies.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---


# Introduktion till App Builder

Historiskt sett har Adobe Commerce-utveckling använt processens utökningsmöjligheter. Processmodellen kräver att ny kod är kompatibel med uppgraderingar, serverns PHP-version och många andra viktiga serverprogram och tjänster som används i Commerce. Adobe Developer App Builder använder sig av processintern utökningsbarhet för att undvika dessa kompatibilitetsproblem.

## App Builder för Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder är en serverlös utökningsplattform för att integrera och skapa anpassade upplevelser för att utöka Adobe-lösningar, och den finns nu för Adobe Commerce. Med App Builder kan ni skapa säkra och skalbara appar som utökar de inbyggda funktionerna i Commerce och integreras med lösningar från tredje part. Som utvecklare kan du nu utnyttja Adobe Commerce utbyggbarhet som inte längre fungerar och som i sin tur ger omedelbara och långsiktiga fördelar.

App Builder erbjuder ett enhetligt ramverk för utbyggbarhet från tredje part för att integrera och skapa anpassade program som utökar [!DNL Adobe Commerce]. Eftersom den här utbyggbarhetsmiljön bygger på Adobe infrastruktur kan utvecklare skapa anpassade mikrotjänster och utöka och integrera [!DNL Adobe Commerce] i andra Adobe-lösningar och integreringar med tredje part.

Med App Builder kan kunderna utöka [!DNL Adobe Commerce] i olika fall:

* Utbyggbarhet för mellanvara - Koppla samman externa system med Adobe-program genom att bygga anpassade anslutningar eller dra nytta av en serie färdiga integreringar.
* utöka bastjänsterna - Utöka de centrala programfunktionerna genom att utöka standardbeteendet med anpassade funktioner och affärslogik.
* användarupplevelsen kan byggas ut - Bygg ut kärnupplevelsen för att uppfylla verksamhetskrav eller bygga kundspecifika digitala resurser, butiker och back-office-applikationer.

Adobe Developer App Builder är en molnbaserad lösning, vilket innebär att den skalas automatiskt. Tjänsten distribueras också globalt för att ge bästa prestanda oavsett var du befinner dig.

## Varför ska du lära dig mer om App Builder?

Eftersom Adobe Commerce inte är en helt SAAS-produkt kan koden som du utvecklar ge problem med komplexitet och uppgradering. Genom att använda icke-processbaserad utbyggbarhet, t.ex. App Builder, kan du tillhandahålla anpassad, unik funktionalitet till din Adobe Commerce-butik utan att behöva använda processmetoder.

Andra fördelar:

* Kopplade funktioner ger snabbare start.
* Uppgraderingar är nu enklare. De anpassade funktionerna ligger utanför Commerce-kodbasen, vilket förhindrar kompatibilitetsproblem vid uppgradering.
* Genom att flytta funktioner och logik utanför Commerce frigörs resurser som normalt används av metoder för pågående utveckling.

## Arkitektur {#architecture}

I stället för en färdig lösning erbjuder Adobe Developer App Builder en gemensam, konsekvent och standardiserad utvecklingsplattform för utökade Adobe Cloud-lösningar som Adobe Commerce, som:

* Adobe Developer Console används för utveckling av anpassade mikrotjänster och tillägg. Bygg och hantera projekt med tillgång till alla verktyg och API:er som behövs för att skapa plugins och integreringar.
* Verktyg, SDK och bibliotek med öppen källkod för att skapa anpassade tillägg och integreringar. Använd React Spectrum (Adobe UI-verktygslådan) om du vill ha ett gemensamt användargränssnitt för alla Adobe-program.
* tjänster som I/O Runtime för värdinfrastruktur på Adobe serverless-plattformen och I/O Events för händelsebaserade integreringar. Adobe har också färdiga funktioner för lagring av data och filer.
* Adobe Experience Cloud där du skickar tillägg och integreringar som ska publiceras i din Experience Cloud-organisation. Systemadministratörer kan granska, hantera och godkänna dessa tillägg. När de har publicerats är dina anpassade tillägg och verktyg i App Builder tillgängliga tillsammans med andra Adobe Experience Cloud-program.

Följande diagram visar hur ett standardprogram som är byggt på App Builder använder dessa funktioner:

![Arkitektur](/help/assets/app-builder/app-builder-architecture.jpeg)

Mer information om App Builder-arkitekturen finns i [Arkitektur - översikt](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Amazon Sales Channel-tillägg {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Utbyggnaden av Amazon Sales Channel är fortfarande under utveckling och har inte officiellt släppts.  Dessa videor och självstudiekurser är avsedda att visa hur du använder Adobe Developer App Builder i ett praktiskt syfte.

I följande självstudiekurser visas hur du ansluter Adobe Commerce till Amazon Sales Channel med ett App Builder-tillägg.

* [teknisk översikt App Builder](../app-builder/app-builder-technical-overview.md)
* [utökningsram](../app-builder/extensibility-framework-commerce-eventing.md)
* [funktionell demonstration av App Builder](../app-builder/app-builder-functional-demonstration.md)

## Kom igång med App Builder {#additional-resources}

En översikt över strategin för sammanställbar e-handel, som innehåller den första konfigurationen, finns i följande blogginlägg:

[Hur App Builder hjälper er att utveckla affärsflexibiliteten för e-handelsplattformen](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Adobe har skapat följande dokumentation för att hjälpa dig komma igång med App Builder:

* [App Builder - komma igång](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Fortsätta inlärningen med dokumentation {#appbuilder-documentation}

App Builder innehåller videor och dokumentation för utvecklare, inklusive guider och referensdokumentation som hjälper dig att utveckla egna program:

* [App Builder-dokumentation](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder-videor](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Testa ett av exempelprogrammen {#appbuilder-codesamples}

Vill du börja utveckla? Följande länk innehåller exempelprogram som hjälper dig att komma igång:

* [App Builder Code Labs på Adobe Developer webbplats](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Support {#support}

För supportärenden för utvecklare använder du [Experience League forum](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} om du behöver hjälp.

{{$include /help/_includes/app-builder-related-links.md}}
