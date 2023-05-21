---
title: Skapa en kundprisregel
description: Lär dig hur du skapar kundprisregler som tillämpar rabatter i kundvagnen baserat på en uppsättning villkor.
doc-type: feature video
audience: all
role: Admin, User
activity: use
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 3ee6eae696d33ce792beabdf91c584e039ab3b54
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---

# Skapa en kundprisregel

Kundvagnsprisregler tillämpar rabatter på artiklar i kundvagnen, baserat på en uppsättning villkor. Rabatten kan tillämpas automatiskt när villkoren är uppfyllda eller när kunden anger en giltig kupongkod. När den tillämpas visas rabatten i kundvagnen under delsumman. En kundvagnsprisregel kan användas efter behov för en säsong eller kampanj genom att ändra dess status och datumintervall.

## Vem är den här videon för?

- Marknadsförare för e-handel
- Webbplatsansvariga

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## Prissättningsproblem

Det finns några unika scenarier som kräver att varje radobjekt visar sin rabatt, men värdena kanske inte matchar exakt. Orsaken är att en kundprisregel ger rabatt på flera produkter men värdena inte fördelas jämnt i två decimaler.

>[!BEGINSHADEBOX]

Prisregel = 10 % rabatt på 2 produkter i kundvagnsvillkoret för att prisregeln ska börja gälla: totalt antal artiklar i kundvagnen är 2 Åtgärder använder procent av produktprisrabatten och rabattbeloppet är 10

2 artiklar läggs till i kundvagnen. Varje är 19,95 USD

För att få rabattbeloppet multiplicerar du produktpriset gånger 0,1

19,95 x 0,1 = 1,995

Detta är problemet, vi har 3 decimaler, istället för två. Att konvertera detta till dollar är nu ett problem

>[!ENDSHADEBOX]

### Lösningen

Med tanke på webbplatsägaren, som är den enda personen som påverkas av denna fråga, bestämdes det att visa varje beställd artikel med rabatten i dollar var den mest lämpliga. För att säkerställa att hela orderbeloppet beräknades korrekt beslutades det att den första posten skulle avrundas och de andra att den tredje decimalen skulle strykas. Granska det här scenariot:

>[!BEGINSHADEBOX]

Samma rabatt på 10 % som ovan gällande kundvagnsregel Lägg till 2 produkter i kundvagnen som är 19,95

Varje produkt bör få 1,995 USD i rabatt Produkt 1 - 19,95 x 0,1 = 1,995 2 - 19,95 x 0,1 = 1,995

Totalsumman 3,99 ges som rabatt till kunden

När radobjekten visas för butiksägaren i administratören måste det första objektet justeras och avrundas upp till 2 000. Det andra objektet vi släpper är den tredje decimalen Produkt 1 = 2.00 Produkt 2 = 1.99

Den totala rabatten för de två produkterna nu när de summeras tillsammans matchar den faktiska rabatten som ges till en kund.
>[!ENDSHADEBOX]

Här är en skärmdump som den skulle visas i administratören för en order som har det här scenariot:

![Administratörsvy som visar beställda artiklar med olika värden](../assets/commerce-admin-cart-price-rule-values-different.png)

### Andra potentiella lösningar och varför de inte användes

>[!BEGINSHADEBOX]

Samma 10% rabatt som ovan varukorgsregel gäller
Lägg till 2 produkter i varukorgen som är 19.95

Varje produkt ska få $ 1.995 i rabatter,
Men om vi bara avrundar dem visar det för mycket rabatt.

Produkt 1 - 19,95 x 0,1 = 1,995 Produkt 2 - 19,95 x 0,1 = 1,995

Konvertera till avrunda alla artiklar Produkt 1 Nytt värde är 2.00 Produkt 2 Nytt värde är 2.00

Det totala beloppet på 3,99 tillhandahölls i själva verket som en rabatt till kunden, men om vi skulle summera det skulle det visa att 4,00 USD gavs, och det är felaktigt.

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

Liknande problem om den tredje decimalen utelämnades för alla artiklar skulle det visa för lite rabatt.

>[!BEGINSHADEBOX]

Samma 10% rabatt som ovan varukorgsregel gäller
Lägg till 2 produkter i varukorgen som är 19.95

Varje produkt ska få $ 1.995 i rabatter, men om vi bara släpper den tredje decimalen händer detta:
Produkt 1 - 19,95 x 0,1 = 1,995
Produkt 2 - 19,95 x 0,1 = 1,995

Konvertera för att släppa den tredje decimalen för alla objekt
Produkt 1 Nytt värde är 1,99
Produkt 2 Nytt värde är 1,99

En totalsumma på 3,99 tillhandahölls faktiskt som rabatt till kunden,
Men om vi släpper den tredje decimalen skulle det visa att $ 3,98 gavs, och det är felaktigt.

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## Ytterligare resurser

- [Skapa en kundprisregel - [!DNL Commerce] Handbok för marknadsföring och reklam](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [Kupongkoder - [!DNL Commerce] Handbok för marknadsföring och reklam](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
