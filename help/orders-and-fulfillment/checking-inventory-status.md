---
title: Lagerstatus kontrollerar utveckling och prestanda
description: Lär dig några idéer och överväganden för inventeringsstatuskontroller för Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
source-git-commit: e171d0a0b6272db5e9ca88992a73395af57072ed
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---


# Lagerstatus kontrollerar utveckling och prestanda

Noggrannhet i inventeringen är mycket viktigt. Det finns vissa inbyggda funktioner som kan säkerställa att den här risken är så låg som möjligt, till exempel återbeställningar och fastställande av utgångsvärdet för lager. Båda dessa ämnen kan läsas vidare [Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) för ytterligare förklaringar.

Det finns projekt och användningsfall där inventeringsstatus i realtid begärs för en Adobe Commerce-butik. Den här självstudiekursen ger dig insikt i hur du hanterar den här konversationen med hänsyn till utveckling och prestanda.

## Validera om denna begäran är absolut nödvändig

Förbered dig för att engagera dig i konversationen med så mycket information som möjligt. Det viktigaste att göra är att verifiera att inbyggd funktionalitet inte är acceptabel för det här projektet. Se skälen till denna begäran om att validera att Adobe Commerce inbyggda funktioner inte uppfyller denna begäran.

En annan faktor är kostnaden för att utveckla, testa och underhålla den här funktionen. Bara för att en del intressenter anser att det är nödvändigt, betyder det inte att det är ett krav. Det kostar mycket att göra lagervalidering utanför Adobe Commerce kärnfunktioner. Dessa kostnader har sin grund i teknisk skuld, mer testning och validering samt dokumentation och styrkande handlingar för dess arkitektur.

## Fastställa vad som är en godkänd lageruppdateringsgräns

Försök att överväga inventeringskontroller och hur de genomförs på tre olika sätt. Alla har sina fördelar och begränsningar. De ökar också komplexiteten och kräver mer testning och har mer att göra med felhanteringen. Kom ihåg att när du bestämmer dig för att gå en anpassad väg finns det nya ansvarsområden och överväganden. Några exempel är en reservprocess, övervakning, testning och felsökning som faller på utvecklingsteamet. Några bra saker att tänka på är ny stöddokumentation, utbildning och övervakning för att se till att utvecklingsteamet kan stödja hela funktionen. En annan bieffekt är att utvecklingsteamet äger hela processen och inte längre utnyttjar de inbyggda funktionerna i Adobe Commerce-programmet. Stödet för Adobe kan inte hjälpa till med den här anpassningsnivån.

Det första sättet är att använda de inbyggda funktionerna. Att använda inbyggd funktionalitet är den minsta risken och har många fördelar. Om du väljer det här sättet kan du förlita dig på all befintlig dokumentation och självstudiekurser som Adobe Commerce tillhandahåller för att använda funktionen. Det finns många aspekter av inventeringshanteringen, så det som kommer med programmet bör vara det första övervägandet. Det finns dock fall där de uppgifter som fanns i handeln vid ordertillfället inte är helt korrekta. Ett exempel på hur det kan bli osynkroniserat är att försäljning tillåts utanför Adobe Commerce-programmet direkt i orderhanteringssystemet. För att säkerställa att rätt lagernivåer finns i Adobe Commerce krävs det någon form av integrering för att informationen om Adobe Commerce ska vara så exakt som möjligt. Om överförsäljning inte är acceptabelt är det en bra metod att lägga till ett värde som ligger utanför lagret för att stoppa försäljningen av artiklar innan du når noll. Den inbyggda synkroniseringsfunktionen för Adobe Commerce är max 1 gång per dag. Detta räcker för vissa användningsfall, men är kanske inte tillräckligt vanligt för andra. Läs [Schemalagd import och export](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) för mer detaljerad information.

Den andra metoden skulle vara `near real-time`. I nästan realtid används fortfarande den inbyggda funktionen. Detta inkluderar dock ett extra arbete som ger en integrering som gör att e-handeln ofta kan uppdatera sitt lager enligt ett schema. Till exempel varje timme. Det här alternativet kräver en viss tanke på hur en integrering kan fungera, men att använda&quot;bulk-API&quot; och ha en viss mellanvara är en bra metod att omvandla data och överföra dem till handeln. Titta på hur du använder Adobe App Builder eller liknande plattformar för att göra större delen av arbetet och skicka informationen till Adobe Commerce på en mer frekvent konferens.

Den tredje metoden och det mest komplexa med störst risk och ansvar är live-inventeringskontroller i realtid till ett externt API eller datakälla. Att göra en inventeringskontroll i realtid i ett externt system är riskabelt och har flera andra element som behöver övervägas. Här är bara några andra saker som behöver utvärderas:

* Kan det externa systemet acceptera REST- eller GraphQL-begäranden
* finns det några begränsningar för slutpunkten, t.ex. X antal begäranden per minut som kanske inte sammanfaller med webbplatstrafiken
* Vad händer med svarstiden under inläsning
* Vad som händer när svarstiderna är långa, avbryter du detta automatiskt och använder ett reservalternativ som det ursprungliga lagret.
* Vilken typ av övervakning är tillgänglig för att se till att API-begäranden ligger inom toleransgränserna

## Överväganden när icke-inbyggd lagerhantering ska övervägas

Behåll anpassningarna så icke-komplexa som möjligt.
Hur platt kan lagerorganisationen vara, är det bara en sku och den totala mängden tillgängligt lager ELLER finns det andra attribut som behöver övervägas.

Om lagerinformationen är ganska platt, till exempel en sku och den totala tillgängliga kvantiteten, utökas alternativen för nästan realtid. Konceptet med nära realtid innebär att det finns en bakgrundsåtgärd som samlar in lagret från källan och sedan fyller i en lagringsmotor som ska användas för att besvara begäran. För detta kan du använda t.ex. Redis, Mongo eller andra databaser som inte är relationella. De här alternativen är bra eftersom de är mycket snabba och fungerar bra för nyckel-/värdepar. Om data är lite mer komplexa krävs troligen en relationsdatabas, antingen inom eller utanför e-handelsprogrammet. Genom att avlasta detta från e-handelsdatabasen håller du affärsprogrammet isolerat från dessa transaktioner. En annan uppsättning fördelar är att spara I/O från e-handelsprogrammet, CPU, RAM och andra från användning. Istället för att använda resurserna från Adobe Commerce programservrar kan du dra nytta av de nya API:erna för att hämta data från lagringsutrymmet utanför webbplatsen.  Detta kommer troligen att kräva en mellanvara för att kunna omvandla data. Kontrollera sedan att det anropande programmet kan få det resultat som förväntat. Genom att använda Adobe App Builder med API-nät kan data omformas och returneras korrekt formaterade.

Att använda Adobe App Builder med API-nät är också ett bra alternativ när det finns flera olika källor för lager.


## Flytta körningslogiken till en plats utanför processen

Adobe Developer App Builder är ett enhetligt ramverk för tredjepartsutbyggbarhet som gör det möjligt att integrera och skapa anpassade upplevelser för att utöka lösningarna från Adobe. Adobe Commerce kan använda Adobe Developer App Builder. Detta skulle vara ett bra sätt att utöka vissa funktioner som normalt finns i kärnprogrammet och som flyttar dem utanför webbplatsen. Genom att ta bort funktioner från Commerce-programmet minskar detta antalet moduler och komplexitet i Commerce-programmet. Lägre antal processanpassade anpassningar minskar i sin tur komplexiteten vid uppgradering och underhåll.

Vi på Adobe har tagit fram dokumentation som kan vara inspiration och innehålla kodexempel. När en kund lägger till en produkt i kundvagnen kontrollerar ett lagerhanteringssystem från en annan leverantör om artikeln finns i lager. Om så är fallet, tillåt du produkten. Annars visar du ett felmeddelande.  För kodexempel och mer information finns på [Webkroks användningsexempel](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## När inventeringskontroller ska utföras

När ska man kontrollera om inventariet fortfarande finns tillgängligt kommer det att skötas av företagets ägare, programvaruarkitekten med synpunkter från andra viktiga intressenter. Ett par lämpliga tillfällen är när du lägger till en artikel i kundvagnen och när du går in i arbetsflödet för utcheckning. Andra händelser lägger bara till belastning i backend-systemen när det inte är nödvändigt. Tänk på att målet är att fånga upp en lagerutleverans endast när den är överflödig. Andra kontroller kan vara bra att ha, men om de kan påverka det övergripande målet för inventeringsstatuskontroller bör de övervägas noggrant och endast tillåtas om affärsaktörerna eller andra är medvetna om den potentiella risken för extra belastning på backend-systemen.

## Undersök hur lagerkällan

Omfattande utredning av den externa inventeringskällan krävs. Objekt som ska utvärderas är tillgängliga API-alternativ, stöd för GraphQL och förväntade svarstider. Om inventeringskällan har en begränsad anslutningsbandbredd eller aldrig var avsedd att användas i realtid, är möjligheten att använda den utesluten och arkitekten måste överväga att använda den nästan i realtid istället.  Om API-begärandetiderna överskrider de definierade parametrarna utesluts även detta från att vara ett möjligt alternativ.  Ett exempel på detta är API-svar som är 200MS® för en enstaka begäran, men som är upp till 500 till 900MS® under medelhög belastning.  Det här skulle troligen bli ännu värre med mer belastning och förhindra att live-inventeringssamtal blir tillgängliga.

Se till att testa API-svarstiderna med enkla förfrågningar och med en hög volym som liknar den förväntade trafiken på den publicerade webbplatsen. Kom ihåg att testa alla delar av handeln samtidigt för att simulera verkliga scenarier. Om förväntningarna är ett direktinventeringsanrop på produktsidorna, när en kundvagn visas och redigeras och under utcheckning, måste lasttestningen simulera alla dessa samtidigt för att efterlikna det verkliga kundbeteendet och förväntningarna på en butik.

## Reservalternativ

Om inventeringskällan är inaktiv och övervakning är tillgänglig rekommenderas att du använder den inbyggda funktionen i Adobe Commerce. Men med korrekt övervakning kan kundupplevelsen ändras dynamiskt för att återspegla förlusten av inventeringskontroller i realtid. Det kan innebära att en försäljning eller ett evenemang avbryts tidigt eller tas bort från skärmen för att undvika överförsäljning. Förväntningarna om vad som ska göras när lagerkällan är nere av någon anledning måste övervägas och diskuteras med butiksägaren så att alla är medvetna om alla automatiska processer som tar över i den olyckliga situationen.

## Slutsats

Beslutet att göra inventeringskontroller i realtid är viktigt. Se till att webbplatsägaren, utvecklingsteamet och andra är fullt utbildade och medvetna om alla vinster och potentiella fallgropar som vilar på utvecklarledaren eller arkitekten. Genom att tillhandahålla en genomtänkt plan som omfattar orsaker och en reservprocess är nyckeln till framgång.

Live-inventeringskontroller kan utföras men kräver forskning och analys av testning och validering under QA-cykeln. Att säkerställa att belastningstestning och helautomatiska tester hjälper till att säkerställa att alla potentiella problem fångas upp och trivs.

Genom att överväga vilka åtgärder som ska utföras om övervakningen upptäcker en trend av misslyckade anrop till det externa systemet eller om svarstiderna överstiger godtagbara trösklar, kan webbplatsen förbli online och erbjuda minsta möjliga irritation för kunderna. Reservalternativen kan vara så enkla som att stänga av de externa inventeringskontrollerna och använda den inbyggda funktionaliteten eller så kan de vara så avancerade som att inaktivera en kampanj, meddela utvecklingsgruppen om de eskalerande problemen eller kanske till och med skicka om förfrågningar till ett andra eller tredje backend-system om de är tillgängliga. Hur reservmekanismen används bör man bara tänka på den faktiska implementeringen eftersom varje integrering har problem vid något tillfälle. Allt som är automatiserat eller behöver manuell åtgärd bör dokumenteras tydligt.
