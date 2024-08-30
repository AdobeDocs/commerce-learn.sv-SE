---
title: Skapa en effektiv supportförfrågan
description: Lär dig mer om hur du skapar en supportanmälan för att maximera effektiviteten i din förfrågan.
feature: Best Practices, Customer Service, Support
topic: Commerce
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-23T00:00:00Z
jira: KT-15165
source-git-commit: b3ec1230e5efa198d71e1a5c0756a47666065117
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# Effektiva supportförfrågningar

När du skapar en supportanmälan är det viktigt att du skickar in den via lämpliga kanaler, ger korrekt och detaljerad information om problemet, väljer rätt organisation och kontaktorsak, väljer lämplig produkt och version, granskar föreslagna artiklar för möjliga lösningar, dubbelkontrollerar all information innan den skickas in, spårar biljettens status och deltar i en konversation med supportteamet, markerar biljetten som löst när problemet är löst och öppnar en uppföljningsbiljett om ytterligare hjälp behövs. &#x200B; Kom ihåg att skicka in biljetten via lämpliga kanaler, tillhandahålla korrekt och detaljerad information, välja rätt organisation och kontaktorsak, välja rätt produkt och version, granska föreslagna artiklar, dubbelkontrollera all information innan den skickas, spåra biljettens förlopp, delta i samtal med supportteamet, markera biljetten som löst när problemet är löst och öppna en uppföljningsbiljett om det behövs. &#x200B;

## Inkludera loggar eller skärmbilder

Det finns flera loggar som är användbara beroende på det aktuella problemet. Om det är möjligt kan du hitta ett avsnitt i loggen och inkludera det som en del av supportförfrågan. Detta avsnitt i loggen hjälper teknikerna att förstå de fel som visas och kommer att snabba upp triageprocessen. Kom ihåg att alla appservrar har individuella loggar och att loggarna matas in i New Relic som ett aggregat.  Det innebär att du kan söka på en plats efter alla fel i stället för att läsa varje programservers enskilda loggfiler. Det sista alternativet är att om det finns en post i New Relic kan en skärmbild också användas som ytterligare information och som hjälper till att snabba upp processen, men du bör ha en NRQL-sats.

## Kontrollera att det finns referenser till tidszonen

Se till att när någon bidrar till ett supportproblem påminna dem om att förtydliga sin tidszon när de hänvisar till en incident. Genom att hänvisa till en tidszon ser man till att när supportteknikteamet undersöker loggar och händelser kan de fokusera på de tidsramar som faktiskt är relevanta. Kom ihåg att någon kan ligga många timmar före eller bakom serverloggtider.

## Beskriv den inledande undersökningen och resultaten

Genom att diskutera och dokumentera alla hittills vidtagna triageåtgärder hjälper supportteknikerna att validera tidiga antaganden. Om det finns stöd för de tre stegen och resultaten kan det snabba upp hela processen. Detta bidrar också till att minska dubbelarbete och i slutänden till att dokumentera resultaten med en valideringsnivå.

## Länkar till New Relic-rapporter eller ange en NRQL-sats

Många av problemen för Adobe Commerce kan spåras via New Relic. Genom att titta på New Relic dashboards eller anpassade NRQL-satser får du insikter om var vissa problem har sitt ursprung. Samma kontrollpaneler och anpassade New Relic-frågor kan delas. Genom att tillhandahålla länkarna i supportbiljetten kan teknikerna se exakt vad reportern är.

>[!MORELIKETHIS]
> 
> - [Adobe Commerce användarhandbok för hjälp](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide){target="_blank"}
