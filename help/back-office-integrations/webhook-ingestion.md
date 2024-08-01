---
title: Konfigurera, driftsätta och anpassa en webkrok för inkommande trafik för integrering av Commerce med ett tredjepartssystem
description: Lär dig hur du konfigurerar och anpassar en webkrok för att underlätta kommunikationen mellan Commerce och ett back office-system från en annan leverantör.
landing-page-description: Lär dig hur du använder Commerce Integration Starter Kit för att integrera Commerce med ett back office-system från tredje part med hjälp av en webkrok för inmatning.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Konfigurera, distribuera och anpassa en webkrok för inmatning

Lär dig mer om hur du konfigurerar och anpassar en webkrok för att integrera Commerce med ett back office-system från tredje part. &#x200B; I den här videon förklaras hur webbhoven kan hantera begränsningar i händelsekommunikation mellan system genom att tillhandahålla en offentligt tillgänglig slutpunkt för att anpassa meddelanden från tredjepartssystemet till Adobe IO Eventing API. Processen innebär att konfigurera webkroken i filen `actions.config.yaml`, aktivera den i filen `app.config.yaml` och distribuera den för att säkerställa korrekt funktionalitet.

I videon beskrivs stegen för hur du ändrar webbkrokkoden för att översätta tredjepartshändelser till format som är kompatibla med integreringens prenumererade händelsetyper. Det handlar om att lägga till en `event-mapping.json`-fil för att underlätta den här översättningen och betonar vikten av att omdistribuera körningsåtgärden efter att ändringarna har gjorts. &#x200B; I videon betonas också vikten av att validera och omvandla inkommande händelsenyttolaster så att de överensstämmer med det förväntade schemat, vilket säkerställer att bearbetningen och integreringen med Commerce-API:t fungerar för att skapa kunder.

## Målgrupp

* Utvecklare som vill skapa en webkrok för inmatning
* Alla som vill anpassa kod för händelseöversättning
* Utvecklare och arkitekter som vill förstå vikten av autentisering och nyttolasthantering

## Videoinnehåll

* Konfiguration och distribution: I videon betonas vikten av att konfigurera webkroken för inkommande trafik i filen `actions.config.yaml` och aktivera den i filen `app.config.yaml`. Det visar också på behovet av att omdistribuera projektet efter att ha gjort ändringar för att säkerställa att webkroken fungerar som den ska.
* Anpassning för kompatibilitet: Det är viktigt att anpassa webbkrokkoden för att översätta tredjepartshändelser till format som överensstämmer med integreringens prenumererade händelsetyper. &#x200B; Denna anpassning säkerställer smidig kommunikation mellan system och lyckad händelsehantering.
* Autentiseringsimplementering: Företag ansvarar för att implementera autentiseringsmekanismer som är lämpliga för deras behov för att förhindra obehöriga förfrågningar när de använder webkroken för förtäring. Detta steg är nödvändigt för att upprätthålla integreringens säkerhet och integritet.
* Validering och transformering av nyttolast: Det är viktigt att verifiera och omvandla inkommande händelsenyttolaster så att de matchar det förväntade schemat för att bearbetningen och integrationen med Commerce API ska lyckas. &#x200B; Genom att trimma och mappa fält på rätt sätt kan integreringen fungera effektivt med nödvändiga data.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Kodexempel

* [Webbkrok för anpassad import](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Lägg till inmatningsplanerare](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
