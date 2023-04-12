---
title: Schema-språk med GraphQL
description: Läs mer om det schema som används i GraphQL. Läs en beskrivning av schemat tillsammans med några intressanta mönster och sätt att läsa schemat.
landing-page-description: Detta är en introduktion till GraphQL. Om schemat och hur vissa element ska tolkas
short-description: Detta är en introduktion till GraphQL. Om schemat och hur vissa element ska tolkas
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Schema-språk

De frågor och mutationer som används är beroende av att ett specifikt datagraf implementeras på servern, som används och används av GraphQL för att lösa frågan. GraphQL-specifikationen definierar ett agnostiskt språk för att uttrycka datagrafens typer och relationer.

Här är ett förkortat typschema som stöder de frågor och mutationer du har tittat på hittills:

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

Du kan ta dig till [GraphQL-dokumentationen](https://graphql.org/learn/schema/){target="_blank"} om du vill veta mer om typsystemets detaljer, inklusive syntax för vissa begrepp som inte finns här. Ovanstående exempel är dock självförklarande. (Observera också hur likartad syntaxen är för att fråga efter syntax.) Att definiera ett GraphQL-schema är bara en fråga om att uttrycka tillgängliga argument och fält av en viss typ, tillsammans med fälttyperna. Varje komplex fälttyp måste ha en definition, och så vidare, genom trädet tills du kommer till enkla skalära typer som `String`.

The `input` deklarationen i alla avseenden är som `type` men definierar en typ som kan användas som indata för ett argument. Observera även `interface` deklaration. Detta fungerar ungefär på samma sätt som gränssnitt i PHP. Andra typer ärver från det här gränssnittet.

Syntaxen `[CartItemInput!]!` i slutänden är den ganska intuitiv. The `!` _inuti_ hakparentesen deklarerar att alla värden i arrayen måste vara icke-null, medan de _utanför_ deklarerar att själva arrayvärdet måste vara icke-null (till exempel en tom array).

>[!NOTE]
>
>Logiken för hur data hämtas och formateras enligt ett schema, och hur sådan logik mappas till särskilda typer, är upp till implementeringen av GraphQL runtime. Implementeringar bör dock följa ett konceptuellt flöde som är rimligt mot bakgrund av en förståelse kring kapslade fält: En lösningsåtgärd som är associerad med roten `Query` eller `Mutation` typ utförs, som undersöker varje fält som anges i begäran. För varje fält som tolkas till en komplex typ utförs en liknande matchning för den typen, och så vidare, tills allt har lösts in i skalära värden.

{{$include /help/_includes/graphql-rest-related-links.md}}
