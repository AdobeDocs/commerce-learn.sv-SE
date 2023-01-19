---
title: Utbyggbarhet utanför processen för Adobe Commerce
description: Lär dig mer om Adobe App Builder och varför det är en viktig aspekt när det gäller utbyggbarhet utanför processen.
landing-page-description: Lär dig vad appverktyget är och hur det kan hjälpa dig med Adobe Commerce utvecklingsstrategier.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-11T00:00:00Z
source-git-commit: ef0fa95e776b97ddbaf30e0acd1340e30f12738f
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 0%

---


# Utbyggbarhet utanför processen

Adobe Commerce-utveckling har historiskt sett gjorts med samma databas som huvudprogrammet.  Detta kallas pågående.  Den här tekniken är mycket bra och ger utvecklaren en förväntad mekanism för att utöka programmet.  Men det här kommer till ett pris.  Varje gång du lägger till ny kod i kodbasen måste den vara kompatibel med alla uppgraderingar.  Du måste också vara kompatibel med servrarna PHP-version och många andra serverprogram och tjänster som e-handeln kommer att använda.  Adobe Developer App Builder har samma krav på att utöka funktionaliteten, men flyttar den från webbplatsen.  Koden och logiken är helt externa och den här metoden kallas icke-processinriktad.

## App Builder för Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder är ett utbyggbart ramverk som utvecklare kan använda för att utöka [!DNL Adobe Commerce] för att tillhandahålla utökningsmöjligheter som inte har bearbetats.

App Builder erbjuder ett enhetligt ramverk för utbyggbarhet från tredje part för att integrera och skapa anpassade program som utökar [!DNL Adobe Commerce]. Eftersom den här utbyggbarhetsmiljön bygger på Adobe infrastruktur kan utvecklare bygga anpassade mikrotjänster samt bygga ut och integrera [!DNL Adobe Commerce] mellan olika Adobe-lösningar och andra tredjepartsintegreringar.

Med App Builder kan kunderna utöka [!DNL Adobe Commerce] i olika fall:

* Utbyggbarhet för mellanvara - Koppla samman externa system med Adobe-program genom att bygga anpassade anslutningar eller dra nytta av en serie färdiga integreringar.
* utöka bastjänsterna - Utöka de centrala programfunktionerna genom att utöka standardbeteendet med anpassade funktioner och affärslogik.
* användarupplevelsen kan byggas ut - Bygg ut kärnupplevelsen för att uppfylla verksamhetskrav eller bygga kundspecifika digitala resurser, butiker och back-office-applikationer.

App Builder (tidigare Project Fireworks) är en molnbaserad lösning, vilket innebär att den skalas automatiskt. Tjänsten distribueras också globalt för att ge bästa prestanda oavsett var du befinner dig.

## Varför ska du lära dig mer om App Builder?

Eftersom Adobe Commerce inte är en fullständig SAAS-lösning kan koden som du utvecklar eller installerar ge problem med komplexitet och uppgradering. Genom att använda icke-processbaserad utbyggbarhet, t.ex. App Builder, kan du tillhandahålla anpassad, unik funktionalitet till din Adobe Commerce-butik utan att behöva använda processmetoder.

Andra fördelar:

* Kopplade funktioner ger snabbare start.
* Uppgraderingar är nu enklare. De anpassade funktionerna ligger utanför e-handelskodbasen, vilket förhindrar kompatibilitetsproblem vid uppgradering.
* Genom att flytta funktioner och logik utanför handeln frigörs resurser som normalt används av metoder för utveckling i processen.

## Arkitektur {#architecture}

I stället för en färdig lösning erbjuder Adobe Developer App Builder en gemensam, konsekvent och standardiserad utvecklingsplattform för utökade Adobe Cloud-lösningar som Adobe Commerce, som:

* Adobe Developer Console används för utveckling av anpassade mikrotjänster och tillägg. Bygg och hantera projekt med tillgång till alla verktyg och API:er som behövs för att skapa plugins och integreringar.
* Verktyg, SDK och bibliotek med öppen källkod för att skapa anpassade tillägg och integreringar. Använd React Spectrum (Adobe UI-verktygslådan) om du vill ha ett gemensamt användargränssnitt för alla Adobe-program.
* tjänster som I/O Runtime för värdinfrastruktur på Adobe serverless-plattformen och I/O Events för händelsebaserade integreringar. Adobe har också färdiga funktioner för lagring av data och filer.
* Adobe Experience Cloud där du skickar tillägg och integreringar som ska publiceras i din Experience Cloud-organisation. Systemadministratörer kan granska, hantera och godkänna dessa tillägg. När de har publicerats är dina anpassade tillägg och verktyg i App Builder tillgängliga tillsammans med andra Adobe Experience Cloud-program.

Följande diagram visar hur ett standardprogram som är byggt på App Builder använder dessa funktioner:

![Arkitektur](/help/assets/app-builder/firefly-architecture.jpeg)

Mer information om App Builder-arkitekturen finns i [Arkitektur - översikt](https://developer.adobe.com/app-builder/docs/guides/).

## Kom igång med App Builder {#additional-resources}

Adobe har skapat följande dokumentation för att hjälpa dig komma igång med App Builder:

* [App Builder - komma igång](https://developer.adobe.com/app-builder/docs/getting_started/)

## Fortsätta inlärningen med dokumentation {#appbuilder-documentation}

App Builder innehåller videor och dokumentation för utvecklare, inklusive guider och referensdokumentation som hjälper dig att utveckla egna program:

* [App Builder-dokumentation](https://developer.adobe.com/app-builder/docs/overview/)
* [App Builder-videor](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## Testa ett av exempelprogrammen {#appbuilder-codesamples}

Vill du börja utveckla? Följande länk innehåller exempelprogram som hjälper dig att komma igång:

* [App Builder Code Labs på Adobe Developer webbplats](https://developer.adobe.com/app-builder/docs/resources/)

## Support {#support}

För supportärenden för utvecklare använder du [Experience League forum](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) om du behöver hjälp.

