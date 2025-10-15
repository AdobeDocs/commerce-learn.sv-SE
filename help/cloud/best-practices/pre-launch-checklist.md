---
title: Adobe Commerce Cloud checklista före start
description: Läs mer om Adobe Commerce Cloud checklista före start.
feature: Cloud
topic: Commerce, Architecture, Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 191cfb29de7b4fff5ca73dcd1603b51d852aebd1
workflow-type: tm+mt
source-wordcount: '1605'
ht-degree: 0%

---

# Checklista för Commerce Cloud före start

Nedan följer en sammanfattning av Adobe Commerce [dokumentation för att starta webbplatsen](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}.

Denna checklista är till för att hjälpa till att planera och genomföra en lyckad start av Adobe Commerce Cloud webbplats. Samarbeta med systemintegratören för Adobe Commerce Cloud för att säkerställa att alla konfigurationsuppgifter och checklisteobjekt är slutförda och verifierade. Om du har problem med några checklisteartiklar eller har frågor kan du kontakta kundens tekniska rådgivare eller Customer Success Engineer. Om ditt konto inte har något tilldelat CTA/CSE kan du skapa ett supportärende för hjälp.

Om du har tilldelats ett CTA/CSE-konto kontaktar du dem och kontohanteraren minst fyra veckor innan du startar den nya Adobe Commerce Cloud-webbplatsen och meddelar dem om din **avsikt** att starta.

- Vissa kontroller är markerade med [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}
- Samarbeta med utvecklare eller systemintegrationspartner för att anpassa er efter er implementeringsstrategi.

>[!IMPORTANT]
> Du accepterar [ansvar](https://experienceleague.adobe.com/sv/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} för eventuella negativa effekter och associerade risker för startschemat för produktionen och pågående stabilitet för webbplatsen om du inte använder och slutför checklistan.

## 1. Live-förarbete

1. Granska dokumentationen om testning och publicering [Dokumentation för att starta webbplatsen](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Se till att en omfattande _&quot;go live readiness plan&quot;_ har förberetts fullständigt med din partner eller systemintegratör, med alla nödvändiga åtgärdsobjekt. Kom ihåg att checklistan före start betonar Adobe bästa praxis, men _&#x200B;**ersätter inte**&#x200B;_ behovet av en egen beredskapsplan.

2. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}[Användarhandbok](https://experienceleague.adobe.com/sv/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. Slutanvändare/handlare utförde UAT (User Acceptance Testing), inklusive backend-operationer.
4. Systemintegratörsteamet har utfört UAT från början till slut på staging och produktion. Mer information finns i [Experience League-dokumentationen](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Bekräfta koddistribution och testning i staging- och produktionsmiljöer ([Läs mer](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. Produktionsklustret har ständigt anpassats till den avtalade dagsbaslinjen. Tala med CTA/CSE om du vill ha mer information, eller ring upp en supportanmälan.

## 2. Aktuella konfigurationer

1. Uppgradera Adobe Commerce och relaterade paket/tjänster till den [senaste versionen](https://experienceleague.adobe.com/sv/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Granska de aktuella konfigurationerna och tjänsterna med din SI/Partner och [följ bästa praxis](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Granska användningen av MySQL/Shared-Files [disk](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## 3. Snabba konfigurationer

1. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}[Helsidescache](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} eller [GraphQL-cachning](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Läs guiden [Konfigurera snabbt](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. Använd metoden GET för GraphQL-frågor på PWA/Headless-webbplatser när det är tillämpligt.

   >[!NOTE]
   > Endast frågor som har skickats in med en HTTP GET-åtgärd kan cachelagras (om tillämpligt). [Det går inte att cachelagra POST-frågor](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Kontrollera att Snabb bildoptimering är aktiverat ([Se Optimering av bilder snabbt](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Kontrollera att rätt sköldplats är konfigurerad ([Konfigurera cache, backends och origin shielding](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Brandväggen för webbaserade program (**WAF**) fungerar. (Se [Felsöka blockerade begäranden](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}, om sådana finns, och begränsningar)
6. Uppdatera listan [&quot;Ignorerade URL-parametrar&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} i administratörspanelen för att förbättra cacheprestanda.

   >[!NOTE]
   > I snabbkonfigurationen under _Admin > Lager > Konfigurationer > System > Fullsidescache > Snabbkonfiguration > Avancerad konfiguration > Avancerad konfiguration > Ignorerade URL-parametrar (Global)_ kan du hitta en kommaavgränsad lista med parametrar som snabbt ska ignoreras vid sökning efter cachelagrade sidor. Glöm inte att överföra VCL:en igen när du har ändrat listan

## 4. DNS och SSL

1. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}_(Skicka en supportanmälan i förväg för alla tillagda eller ändrade domäner)_
2. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}[den här artikeln](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} om du vill ha mer information.
3. Uppdatera DNS-värdet [TTL (Time to Live)](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} så långt det går, för live.
4. Aktivera Sendgrid SPF och DKIM

   >[!NOTE]
   > Lägg till SendGrid CNAME-posterna för varje domän i DNS-konfigurationen. Läs [e-posttjänsten SendGrid](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} om du vill veta hur du ändrar avsändardomäner och mycket mer.

## 5. Databaskonfigurationer

Adobe Commerce Cloud använder ett MariaDB Galera-kluster som databas för både testnings- och produktionsmiljöer. Galerakluster är avgörande för att förbättra prestanda och skalbarhet. Läs följande artiklar om du vill få insikter i hur Galera-klusterreplikeringar fungerar optimalt.

- [Bästa praxis för MySQL-konfigurationer](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Hanterade aviseringar på Adobe Commerce: [MariaDB-aviseringar](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- Bästa tillvägagångssätt för [databaskonfiguration](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- Djupanalys till [Galera-klusterreplikeringar och flödeskontroll.](https://experienceleague.adobe.com/sv/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. [MYSQL Slave-anslutning](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} rekommenderas för bättre prestanda vid hög databasinläsning.
2. Se till att radformatet för alla databastabeller är inställt på [DYNAMIC i stället för COMPACT](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (Detta gäller särskilt för migreringar på plats till molnet).
3. Ändra databasens lagringsmotor från [MyISAM till InnoDB](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} för alla tabeller.
4. Granska och optimera databastabeller som överskrider 1 GB i storlek i god tid.
5. Databasschemainformationen är aktuell och aktuell. (Se [den här handboken](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## 6. Distributioner

1. Granska det idealiska läget för statisk innehållsdistribution (SCD) för att minska underhållstiden under distributioner i produktionsmiljön. Granska guiden [Statisk innehållsdistribution (SCD)](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} och [Butikskonfigurationshantering](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}.
2. Granska minifilinställningarna för HTML, JavaScript och CSS. (Detta gäller inte PWA/Headless-webbplatser).
3. Bekräfta att användningen av följande molnvariabler överensstämmer med deras avsedda ändamål. ([SCD_MATRIX](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} och [SKIP_SCD](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## 7. Testning och felsökning

1. Testa utgående transaktionsmejl. Läs mer om funktionen [Adobe Commerce Cloud - SendGrid Mail](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}
3. [!BADGE Blockerare]{type=caution tooltip="Potentiell blockerare"}

   >[!NOTE]
   > Ett [belastnings- och stresstest har till syfte &#x200B;](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} att identifiera flaskhalsar och upptäcka prestandaproblem i programmet. Det spelar en viktig roll när det gäller att hantera förväntningar på klusterstorlek och fastställa nödvändiga skalningsjusteringar för att effektivt kunna uppfylla verksamhetskraven.

   >[!IMPORTANT]
   > **_VARNING!_** Vid förberedelse av ett inläsningstest ska du **_inte_** skicka ut e-postmeddelanden med live-transaktioner (även till dummy-adresser). Om du skickar e-postmeddelanden under testningen kan projektet nå den standardgräns för sändning (12 kB) som konfigurerats för SendGrid innan det startas.
   > 
   > - Så här inaktiverar du e-postkommunikation:
   >   Gå till _Store > Konfiguration > Avancerat > System > Inställningar för e-postsändning_.

4. Utför säkerhetspenetrationstestning på produktionsinstansen som en del av säkerhetsmodellen [för delat ansvar](https://business.adobe.com/se/products/magento/secure-ecommerce.html){target="_blank"}. För PCI-kompatibilitet (betalkortsbranschen) kräver den anpassade platsen penetrationstestning.

## 8. Andra konfigurationer

1. Växla indexering till _&quot;uppdatera enligt schema_&quot;, förutom **_customer_grid_** som finns kvar på&quot;SPARA&quot; (se [Indexeringslägen](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Använder du sökmotorer eller tillägg från tredje part?
3. Bekräfta att konfigurationerna för [SEO (sökmotoroptimering) är korrekt konfigurerade](https://experienceleague.adobe.com/sv/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} så att indexerare/crawler kan skanna webbplatsen, om det är relevant.
4. Lägg till omdirigeringar och vägar (se [Konfigurera vägar](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   >Lägg till omdirigeringar och vägar till filen route.yaml i integreringsmiljön och verifiera konfigurationen i den här miljön innan du distribuerar till Förproduktion.

       &quot;http://{all}/&quot;:
       type: upstream
       upstream: &quot;mymagento:http&quot;
       
       &quot;http://{all}/&quot;:
       type: upstream 
       upstream: &quot;mymagento:http&quot;
   
5. Kontrollera att XDebug är inaktiverat om det aktiveras under utvecklingen (se [Konfigurera Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}).
6. Kontrollera att op-cache och andra konfigurationer har uppdaterats korrekt i php.ini-filen ([referera till det här exemplet](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Prenumerera på [**Adobe Commerce statussida**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. Prenumerera på New Relic [Managed Alerts för Adobe Commerce](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}-meddelandekanaler för att övervaka angivna prestandamått ([läs mer](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## 9. Säkerhet

1. Konfigurera Adobe Commerce Security Scan

   >[!NOTE]
   > [Adobe Commerce Security Scan är ett användbart verktyg](https://experienceleague.adobe.com/sv/docs/commerce-admin/systems/security/security-scan){target="_blank"} som hjälper till att upptäcka gamla programversioner, felaktig konfiguration och eventuell skadlig kod på webbplatsen. Registrera dig, schemalägg det så att det körs ofta och se till att e-postmeddelanden skickas till rätt kontaktperson inom teknisk säkerhet.
   > 
   > Slutför den här uppgiften under UAT. Om du använder alternativet för periodisk skanning måste du schemalägga skanningar vid låg efterfrågan. Se sidan [Säkerhetskontroll](https://account.magento.com/scanner/index/dashboard/){target="_blank"} i Adobe Commerce-kontot. Du måste logga in på ett Adobe Commerce-konto för att komma åt säkerhetsgenomsökningen.

2. Ändra standardinställningarna för Adobe Commerce Admin.
3. Ändra administratörslösenordet (se [Konfigurera administratörsskydd](https://experienceleague.adobe.com/sv/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Ändra admin-URL:en (se [Använda en anpassad admin-URL](https://experienceleague.adobe.com/sv/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Ta bort användare som inte längre är med i projektet (se [Skapa och hantera användare](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}).
6. Lösenord för administratörer har konfigurerats (se [Krav för administratörslösenord](https://experienceleague.adobe.com/sv/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Konfigurera tvåfaktorautentisering (se [Tvåfaktorautentisering](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}).

## 10. Go Live

Utför följande steg när det är dags att klippa över (mer information finns i [DNS-konfigurationer](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}):

1. Få åtkomst till din DNS-tjänst och uppdatera A- och CNAME-poster för var och en av dina domäner och värdnamn:
   1. Lägg till en CNAME-post för _&lt;&lt;www.yourdomain.com>_ som pekar på **prod.magentocloud.map.fastly.net**
   2. Ange fyra A-poster för _&lt;&lt;dindomän.com>_ som pekar på:\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Ändra Adobe Commerce bas-URL till _&lt;&lt;www.yourdomain.com>>_
3. Vänta tills TTL-tiden har gått och starta sedan om webbläsaren.
4. Testa webbplatsen.

### Om du har problem med att blockera live-erbjudandet:

Om du råkar ut för problem som hindrar dig från att starta under klippet är det snabbaste sättet att få rätt support i rätt tid att använda helpdesk och öppna en biljett med anledningen&quot;Det går inte att starta min butik&quot; och ringa ett supportnummer (se [listan över hotline-nummer för Adobe Commerce P1 (prioritet 1) &#x200B;](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}):

- Amerikanskt avgiftsfritt: (+1) 877 282 7436 (direkt till Adobe Commerce P1 hotline)
- Amerikanskt avgiftsfritt: (+1) 800 685 3620 (På första menyn trycker du på 7 för Adobe Commerce P1 hotline)
- US Local: (+1) 408 537 8777

## 11. Post Go-Live

När webbplatsen är klar skickar du ett e-postmeddelande till den tilldelade CTA (Customer Technical Advisory), CSE (Customer Success Engineer) och AM (Account Manager). Om du inte har någon kontoansvarig tilldelad till projektet kan du skapa en supportanmälan som ber om att High SLA Monitoring ska aktiveras när webbplatsen har publicerats. CTA/CSE utför följande uppgifter så snart sajten har bekräftats startas med Snabbaktiverad och cachelagring:

- Tagga klustret som live och skapa en supportbiljett för att aktivera övervakning av High SLA (Service Level Agreements).
- Aktivera New Relic Synthetics för övervakning av drifttid.

>[!MORELIKETHIS]
> 
> - [Starta beredskapsöversikt - Implementeringsspelbok](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [Startchecklista - användarhandbok](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Förhandsstarta checklista - Webbplatshanteraren/Commerce Admin&#39;s Guide](https://experienceleague.adobe.com/sv/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [Säkerhetsmodell för delat ansvar](https://experienceleague.adobe.com/sv/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
