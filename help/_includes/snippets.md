---
title: Edition Banners
description: Återanvända visuella element för anteckningsfunktion eller sidor som gäller en viss utgåva
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Edition Banners

## Endast EE-funktion {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Funktionen Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Endast exklusiv funktion i Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=sv-SE#product-editions">Läs mer</a>)</td></tr>
</table>

## Endast B2B-funktion {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Funktionen Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Exklusiv funktion endast tillgänglig med <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html?lang=sv-SE">B2B för Adobe Commerce</a></td></tr>
</table>

## 400 problem {#avoid-400-error}

>[!CAUTION]
>
>När du utför API-anrop måste du se till att någon typ av searchCriteria används. Du kan också överväga att använda sidnumrering. Om resultatet från Adobe Commerce är för stort kan Adobe Developer App Builder-kapaciteten uppfyllas och filen avslutas oväntat. Resultatet är ett felaktigt svarsresultat som ett 400-fel.\
> Anta till exempel att du behöver begära alla aktuella produkter från Adobe Commerce. Den resulterande URL:en liknar `{{base_url}}rest/V1/products?searchCriteria=all`. Beroende på storleken på katalogen som returneras kan json vara för stor för att App Builder ska kunna använda den. Använd sidnumrering i stället och gör några förfrågningar för att undvika `Response is not valid 'message/http'.`
