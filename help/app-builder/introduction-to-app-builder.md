---
title: Utbyggbarhet utanför processen för Adobe Commerce
description: Lär dig mer om Adobe App Builder och varför det är en viktig aspekt av utökningsbarhet utanför processen.
landing-page-description: Lär dig vad App Builder är och hur det kan hjälpa dig med utvecklingsstrategier för Adobe Commerce.
short-description: Lär dig vad App Builder är och hur det kan hjälpa dig med utvecklingsstrategier för Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 6%

---

# Introduktion till App Builder

Historiskt sett har Adobe Commerce-utveckling använt processens utökningsmöjligheter. Processmodellen kräver att ny kod är kompatibel med uppgraderingar, serverns PHP-version och många andra viktiga serverprogram och tjänster som Commerce använder. Adobe Developer App Builder använder icke-processbaserad utbyggbarhet för att undvika dessa kompatibilitetsproblem.

## App Builder för Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder är en serverlös utbyggbar plattform för att integrera och skapa anpassade upplevelser för att utöka Adobe lösningar, och den finns nu för Adobe Commerce. Med App Builder kan ni skapa säkra och skalbara appar som utökar Commerce inbyggda funktioner och integreras med tredjepartslösningar. Som utvecklare kan du nu utnyttja Adobe Commerce utbyggbarhet som inte längre fungerar och som i sin tur ger omedelbara och långsiktiga fördelar.

App Builder tillhandahåller ett enhetligt ramverk för utbyggbarhet från tredje part för integrering och skapande av anpassade program som utökar [!DNL Adobe Commerce]. Eftersom det här utökningsramverket bygger på Adobe infrastruktur kan utvecklare skapa anpassade mikrotjänster och utöka och integrera [!DNL Adobe Commerce] i andra Adobe-lösningar och tredjepartsintegreringar.

App Builder erbjuder ett sätt för kunder att utöka [!DNL Adobe Commerce] i olika fall:

* Utbyggbarhet för mellanvara - Koppla samman externa system med Adobe-program genom att bygga anpassade anslutningar eller dra nytta av en serie färdiga integreringar.
* utöka bastjänsterna - Utöka de centrala programfunktionerna genom att utöka standardbeteendet med anpassade funktioner och affärslogik.
* användarupplevelsen kan byggas ut - Bygg ut kärnupplevelsen för att uppfylla verksamhetskrav eller bygga kundspecifika digitala resurser, butiker och back-office-applikationer.

Adobe Developer App Builder är en molnbaserad lösning, vilket innebär att den skalas automatiskt. Tjänsten distribueras också globalt för att ge bästa prestanda oavsett var du befinner dig.

## Varför ska du lära dig mer om App Builder?

Eftersom Adobe Commerce inte är en helt SAAS-produkt kan koden som du utvecklar ge problem med komplexitet och uppgradering. Genom att använda icke-processbaserad utbyggbarhet, som App Builder, kan du tillhandahålla anpassad, unik funktionalitet till din Adobe Commerce-butik utan att behöva använda processmetoder.

Andra fördelar:

* Kopplade funktioner ger snabbare start.
* Uppgraderingar är nu enklare. De anpassade funktionerna ligger utanför Commerce kodbas, vilket förhindrar kompatibilitetsproblem vid uppgradering.
* Genom att flytta funktioner och logik utanför Commerce frigörs resurser som normalt används av metoder för pågående utveckling.

## Arkitektur {#architecture}

Istället för en körklar lösning erbjuder Adobe Developer App Builder en gemensam, enhetlig och standardiserad utvecklingsplattform för utbyggnad av Adobe Cloud-lösningar som Adobe Commerce:

* Adobe Developer Console används för utveckling av anpassade mikrotjänster och tillägg. Bygg och hantera projekt med tillgång till alla verktyg och API:er som behövs för att skapa plugins och integreringar.
* Verktyg, SDK:er och bibliotek med öppen källkod för att skapa anpassade tillägg och integreringar. Använd React Spectrum (Adobe UI Toolkit) för att ha ett gemensamt användargränssnitt för alla Adobe-program.
* tjänster som I/O Runtime för värdinfrastruktur på Adobe serverlösa plattform och I/O Events för händelsebaserade integreringar. Adobe har också färdiga funktioner för lagring av data och filer.
* Adobe Experience Cloud där du skickar tillägg och integreringar som ska publiceras i din Experience Cloud-organisation. Systemadministratörer kan granska, hantera och godkänna dessa tillägg. När de har publicerats är dina anpassade App Builder-tillägg och verktyg tillgängliga tillsammans med andra Adobe Experience Cloud-program.

Följande diagram visar hur ett standardprogram som bygger på App Builder använder dessa funktioner:

![Arkitektur](/help/assets/app-builder/app-builder-architecture.jpeg)

Mer information om App Builder-arkitekturen finns i [Översikt över arkitekturen](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Kom igång med App Builder {#additional-resources}

En översikt över strategin för sammanställbar e-handel, som innehåller den första konfigurationen, finns i följande blogginlägg:

[Hur App Builder kan göra verksamheten smidigare för e-handelsplattformen](https://business.adobe.com/se/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

För att du ska komma igång med App Builder har Adobe skapat följande dokumentation:

* [App Builder Komma igång](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Fortsätta inlärningen med dokumentation {#appbuilder-documentation}

App Builder tillhandahåller videor och dokumentation för utvecklare, inklusive guider och referensdokumentation som hjälper dig att utveckla egna program:

* [App Builder-dokumentation](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder-videor](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Testa ett av exempelprogrammen {#appbuilder-codesamples}

Vill du börja utveckla? Följande länk innehåller exempelprogram som hjälper dig att komma igång:

* [App Builder Code Labs på Adobe Developer webbplats](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Support {#support}

Använd [Experience League-forumet](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly?profile.language=sv){target="_blank"} för att få hjälp med supportförfrågningar från utvecklare.

{{$include /help/_includes/app-builder-related-links.md}}
