---
title: Upptäck nya REST API:er för kunder
description: Upptäck hur du använder nya REST API:er för kunder i tjänsten Adobe Commerce Cloud. Idealiskt för arkitekter och utvecklare.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: cb3fecf5f8b23425311dc31ed526b3b9bfe07b45
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# Kundens REST API

Lär dig använda nya REST API:er för kunder i Adobe Commerce as a Cloud Service. Den här självstudiekursen är perfekt för arkitekter och utvecklare som vill integrera och optimera API-lösningar effektivt.

## Vem är den här videon till?

* Utvecklare som ansvarar för integreringen med Adobe Commerce
* Tekniska arkitekter som utformar arbetsflöden för kundhantering för headless commerce-implementationer

## Videoinnehåll

* Autentisera med Adobe IMS med autentiseringsuppgifter för server-till-server för att få en åtkomsttoken för API-begäranden
* Använd rätt REST API-slutpunktsformat för Commerce as a Cloud Service
* Skapa och uppdatera kundkonton programmatiskt med POST- och PUT-begäranden med rätt JSON-nyttolaster

>[!VIDEO](https://video.tv.adobe.com/v/3479361/?learn=on&enablevpops)

## Kodexempel

Innan du startar samlar du in alla värden som krävs från [Experience Cloud](https://experience.adobe.com) och [Adobe Developer Console](https://developer.adobe.com/console). Med dessa värden klara får du en smidig konfigurationsprocess.

>[!NOTE]
>
>Se till att du arbetar i rätt organisation. Ditt val av organisation påverkar vilka instanser och miljöer som visas i både Experience Cloud och Developer Console.

### Instansinformation - experience.adobe.com

Instansinformationen innehåller saker som ditt Instance ID, GraphQL-slutpunkter och autentiseringsuppgifter.

### Information om utvecklare - https://developer.adobe.com/console/

I Developer Console hanterar du dina API-autentiseringsuppgifter, inklusive klient-ID:n, klienthemligheter och åtkomsttoken. Du kan också skapa nya autentiseringstyper, till exempel Server-till-server- eller Native App-autentisering.

## Förutsättningar

| Objekt | Värde | Var är det här värdet? |
|--- |--- |--- |
| Instans-ID | `<instance_id>` | experience.adobe.com |
| REST-slutpunkt | `<rest_endpoint>` | experience.adobe.com |
| Klient-ID | `<client_id>` | developer.adobe.com/console |
| Klienthemlighet | `<client_secret>` | developer.adobe.com/console |


## Steg 1: Hämta åtkomsttoken (server-till-server-autentisering)

>[!IMPORTANT]
>
> Variablerna som visas i det här exemplet är ogiltiga. Använd &lt;client_id> och &lt;client_secrets> från dina projektuppgifter.

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**Exempelsvar:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## Steg 2: Skapa en kund

>[!IMPORTANT]
>
> Den URL som anges i det här exemplet är inte giltig. Använd din REST-bas-URL. Exchange &#39;&lt;rest_endpoint>&#39; med din URL. Det ser ut ungefär som denna `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.
>
> Den här slutpunkten har inte /rest/ som en del av URL:en. Om du tar med det leder det till ett fel.

**Slutpunkt:** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**Svar:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## Steg 3: Uppdatera en kund

>[!IMPORTANT]
>
> Den URL som anges i det här exemplet är inte giltig. Använd din REST-bas-URL. Exchange &#39;&lt;rest_endpoint>&#39; med din URL. Det ser ut ungefär som denna `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.

Numret `5` i följande exempel är ID:t från den tidigare skapade kunden med POST `"id": 5,`. Ändra `5` till det ID som returnerades i din begäran.

**Slutpunkt:** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**Svar:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## Fullständigt skript (allt-i-ett)

>[!IMPORTANT]
>
> Variablerna som visas i det här exemplet är ogiltiga. Använd klient-ID och klienthemlighet från dina projektuppgifter. Använd din REST-bas-URL. Exchange &#39;&lt;rest_endpoint>&#39; med URL:en för REST-slutpunkten från experience.adobe.com. Det ser ut ungefär som denna `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## Viktiga anteckningar om den här självstudien

1. **URL-sökväg**: Använd `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **Autentisering**: I den här självstudiekursen användes Server-till-server (`client_credentials` anslagstyp)
1. **Obligatoriskt scope**: `commerce.accs`
1. **Token förfaller**: 86400 sekunder (24 timmar)

## Referenser

* [Versionsinformation för Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/sv/docs/commerce/cloud-service/release-notes)
* [SaaS REST API-referens](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [Handbok för användarautentisering](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
