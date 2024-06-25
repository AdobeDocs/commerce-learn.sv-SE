---
title: Anslut och köra frågor mot databasen
description: Lär dig flera metoder för att ansluta till ett Adobe Commerce molnprojekt. Lär dig hur du drar ned en databas för användning utanför webbplatsen. Lär dig några metoder för att maskera PII och ta bort det.
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-06-25T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: 435364592c0b609b3c379bb58df80e2691c82d40
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---

# Ansluta och köra frågor mot Adobe Commerce-databasen

Lär dig hur du ansluter till ett Adobe Commerce i ett molnprojekt, skapar en databassdump för användning på annan plats och hanterar PII (Personally Identiitable Information) genom att maskera eller ta bort den. Lär dig hur du får åtkomst till Adobe Commerce-data på olika sätt, bland annat lokala DB-dumpar, fjärrdatabasanslutningar med program som MySQL Workbench eller TablesPlus, och direktanslutningar via CLI-verktyget i Magento Cloud.

## Videoinnehåll

* Lär dig hur du snabbt ansluter till ett Adobe Commerce Cloud-fjärrprojekt med ett verktyg som MysqlWorkbench eller TablesPlus.
* Lär dig hur du snabbt ansluter till Adobe Commerce-projektet för att köra SQL via kommandoraden

>[!VIDEO](https://video.tv.adobe.com/v/3430507?learn=on)

Lär dig hur du ansluter till ett Adobe Commerce i ett molnprojekt, dumpar en databas för användning utanför webbplatsen och maskerar PII och tar bort den.

Du kan komma åt Adobe Commerce-data från ditt molnprojekt på något av följande sätt:

* Använda en lokal DB-dump
* En DB-anslutning till fjärrmolnmiljön med ett program som Mysql Workbench eller Tables Plus
* Ansluta direkt till molnmiljön med hjälp av CLI-verktyget i magento-cloud och kör kommandon på fjärrservern

Den bästa metoden är att göra en databasdump och svepa den för att ta bort all kundinformation. Ta bort kunddata helt om data inte behövs.

## Använda Adobe Commerce Cloud CLI-verktyget

Om du vill skapa en databasdump måste du ha [ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) installerade. Gå till en katalog på din lokala bärbara dator och kör följande kommando. Se till att ersätta `your-project-id` med projekt-ID, som liknar `asasdasd45q`. Du måste även ersätta `your-environment-name` med namnet på miljön, till exempel `master` eller `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

Om du inte är säker på projekt-ID:t eller miljön kan du utelämna dessa i kommandot:

`magento-cloud db:dump`

CLI ber dig specificera rätt projekt och miljö. I följande exempel visas den dialogrutan. I det här exemplet visas flera projekt som har tilldelats ditt konto, men du har förmodligen bara ett projekt tillgängligt.

Byta till en katalog

```bash
cd ~/Downloads/db-tutorial 
```

Kör nu kommandot för att skapa databasdumpen

```bash
magento-cloud db:dump
```

Eftersom vi inte har angett något projekt eller någon miljö ställer Adobe Commerce CLI några frågor, här är några exempel på dialogrutor

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Använda Adobe Commerce ECE-verktygen

Om du inte har Adobe Commerce CLI-verktyget kan du `ssh` i projektet och kör `ece` kommando `vendor/bin/ece-tools db-dump`: Samplingssvar:

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

Använd `SFTP` eller `rsync` för att dra databasdumpen till din lokala miljö.

I följande exempel används `rsync` för att dra filen till `~/Downloads/db-tutorial` mapp.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

Terminalfönstret kommer att visa viss information, här kommer några exempel på utdata

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

Visa innehållet i filen för att verifiera att den har hämtats.

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

När du har fått informationen ska du se till att rensa den genom att ta bort eller maskera kunddata. Följande exempelskript hjälper dig att komma igång.

I det här exemplet omvandlas kunddata till slumpmässiga strängar, men alla objekt behålls. Det här exemplet innehåller några extra tabeller som visar att kundens PII-kod kan hittas både i tredjepartstabeller och i kärntabeller. Granska noggrant data i alla tabeller och maskera eller ta bort alla kunddata.

Vanligtvis är arkitekten eller huvudutvecklaren den enda person som ansvarar för maskering och sanering av databasdumpar. Genom att ha en dedikerad saneringsleverantör minskar exponeringen för rådata, vilket minskar möjligheterna att bryta mot gällande regler och bestämmelser.

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

Du kan också ta bort posterna i stället för att maskera informationen, vilket även gör den nya databasen mindre. När PII-filen maskerats eller tagits bort kan data tryggt skickas till en teamkamera för användning i den lokala miljön.

## Fjärrdatabasanslutning till ett Adobe Commerce Cloud-projekt

Den här metoden tillåter inte oavsiktlig redigering och borttagning av verkliga data. Detta tillvägagångssätt bör användas med försiktighet. Att använda en säkerhetskopiering och att granska data offline är det bästa sättet. Det finns tillfällen då man behöver få tillgång till data direkt på Adobe Commerce Cloud, men det är riskabelt. Det finns inget &quot;är du säker?&quot; frågor och svar, så det är möjligt att ändra eller ta bort data av misstag.

Superviktigt! Det är bekvämt att göra en fjärransluten DB-anslutning och använda riktiga livedata, men det innebär en risk. Personligen och som Principal Technical Architect för Adobe Commerce rekommenderar jag det inte. Det är för lätt att glömma att du finns i fjärrdatabasen och av misstag ta bort eller ändra data. Det finns ett alternativ för att ansluta till den skrivskyddade repliken, men det ger en viss effekt på platsen beroende på hur stora SQL-aktiviteterna är. Men eftersom det är möjligt är detta de steg som krävs för att uppnå det.

Upprätta en SSH-tunnel:

```bash
magento-cloud tunnel:open
```

När projektet har valts och miljön har valts, finns det utdata från det kommando som används i inställningarna för det grafiska gränssnittet mysql.

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

Upprätta en anslutning med hjälp av ett grafiskt MySQL-gränssnitt med `SSH tunnel opened to database at` kommandoalternativ.

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

Nu när du har rätt information kan du fortsätta att infoga dessa värden i molnkonsolen.

Du hittar SSH-värdnamnet och användarnamnet från molnautentiseringsuppgifterna i molnkonsolen.

![logotyp - Adobe Commerce Cloud Console](./assets/cloud-ui-screenshot.png "Adobe Commerce Cloud Console")

Här är ett exempel: `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
SSH-värdnamnet är allt efter @-tecknet: `ssh.us-4.magento.cloud` i detta exempel.
SSH-användarnamnet är allt före @-tecknet:  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## Söker efter värden att ansluta till databasen

För att få åtkomst till MariaDB-databasen direkt måste SSH användas för att logga in i fjärrmolnmiljön och ansluta till databasen.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Hämta inloggningsuppgifterna för MySQL från `database` och `type` egenskaper i [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) variabel.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   eller

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Leta reda på MySQL-informationen i svaret. Exempel:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

Använd sedan konfigurationsvärdena i ditt MySQL-GUI. I följande exempel används MySQL Workbench, men alla program som har stöd för MySQL-anslutningar har liknande fält.

![logo - Exempel på Mysql GUI med Mysql Workbench](./assets/mysql-workbench-after-connecting.png " Exempel på Mysql GUI som använder Mysql Workbench")

![logo - Exempel på Mysql GUI med TablesPlus](./assets/tablesPlus-db-connection.png " Exempel på Mysql GUI med TablesPlus")

När allt har konfigurerats går det att använda ett MySQL-GUI för att köra frågor i ett Adobe Commerce Cloud-fjärrprojekt.

## Ansluta direkt till molnprojektdatabasen för att köra SQL

Följande metod använder `magento-cloud` cli för att ansluta direkt till mysql-databasen och köra SQL, vilket ger snabbare databasfrågor. Om du behöver kopiera den här databasen kan du gå till någon av de alternativa metoderna för att [skapa en databasdump](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Du kan till exempel hitta alla poster från `core_config_data` tabell som innehåller ordet `secure` som en del av kolumnen `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## Ytterligare resurser

[ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[Konfigurera tjänsten MySQL](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[Konfigurera en MySQL-fjärrdatabasanslutning](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[Skapa databasdump på Adobe Commerce i molninfrastrukturen](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
