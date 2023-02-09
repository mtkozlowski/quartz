---
date: 2022-04-25
tags: courses/ArchitekturaNaFroncie 
---
# Unie dyskryminacyjne w Typescript

Jak wspomniałem [[Unia i przecięcie typów w Typescript]], kompilator jest w stanie zagwarantować to, czego jest pewny po unii typów (zanim rozstrzygnie z którym dokładnie typem ma doczynienia).

Czasem zdarza się, że chcemy wykorzystać unię typów, których pola nie do końca się pokrywają. Musimy w skrypcie być pewni, z czym mamy do czynienia.

Z pomocą przychodzi pole, które będzie wspólne dla wszystkich typów w unii, np. `type`. Dzięki niemu będziemy mogli sprawdzić za pomocą switcha lub instrukcji warunkowej, co dokładnie to za typ.

W poniższym przykładzie takim polem będzie pole `CountryCode`. Przeskocz do [[Unie dyskryminacyjne w Typescript#Rozwiązanie]]

### Wersja błędna

```Typescript
// do czego zdolne są unie 😱
// możemy w czasie kompilacji zwalidować poprawność danych
// ALE
// muszą być literałami - muszą być znane w czasie kompilacji
// (jak będą znane dopiero w runtime, to za późno - wiadomo)

type Country = "PL" | "DE" | "FR" | "UK";

type Tax_PL = 0 | 0.05 | 0.08 | 0.23; // 0%, 5%, 8%, 23%
type Tax_DE = 0 | 0.07 | 0.19; // 0%, 7%, 19%
type Tax_FR = 0 | 0.021 | 0.055 | 0.1 | 0.2; // 0%, 2.1%, 5.5%, 10%, 20%
type Tax_UK = 0 | 0.05 | 0.2; // 0%, 5%, 20%

// ten typ jest za szeroki...
// bo pozwala na połączenie dowolnej stawki podatku z dowolnym krajem
// a chcemy, aby dopuszczalne były tylko te

type Purchase = {
  country: Country;
  vatTax: number;
  name: string;
  netPrice: number;
};

const purchases: Purchase[] = [
  {
    country: "PL",
    vatTax: 0.055,
    name: "dumplings",
    netPrice: 100,
  },
  {
    country: "DE",
    vatTax: 0.1,
    name: "bawarian beer",
    netPrice: 200,
  },
];
```

### Rozwiązanie

Skorzystanie z unii i przecięcia daje pewność, że dla danego kodu państwa będziemy mogli wybrać tylko właściwe wartości podatku.

```Typescript
type CountryTaxes =
  | { CountryTag: "PL", CountryTaxes: Tax_PL }
  | { CountryTag: "DE", CountryTaxes: Tax_DE }
  | { CountryTag: "FR", CountryTaxes: Tax_FR }
  | { CountryTag: "UK", CountryTaxes: Tax_UK };

type PurchaseItem = CountryTaxes & { name: string, netPrice: number };

const purchaseItems: PurchaseItem[] = [
  {
    CountryTag: "FR",
    CountryTaxes: 0.055,
    name: "dumplings",
    netPrice: 100,
  },
]
```