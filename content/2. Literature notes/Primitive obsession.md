---
date: 2022-04-15
tags: [courses/ArchitekturaNaFroncie]
---
# Primitive obsession

Nadużywanie typów prymitywnych sprawia, że np. wszystkie liczby w aplikacji są ze sobą kompatybilne, co sprawia, że:
- możemy dodać do siebie pieniądze i liczbę kół w aucie
- możemy pomnożyć ze sobą dwie zmienne przechowywujące pieniądz i otrzymać w teorii $pieniądz^2$
- i inne babole

## Rozwiązanie:

Do wyboru mamy dwa rozwiązania
- [[#Opaque Brand types]], które są mniej uciążliwe w pisaniu i skutkują mniejszym obciążeniem w Runtime, ale nie narzucają na programistę wysokiej dyscypliny
- [[#Value objects]], które skutkują wyższym narzutem programistycznym i Runtime'owym, ale zapewniają większą kotrolę w przyszłości

### Opaque/Brand types

Rozszerzamy typ lub interfejs bez wpływania na to, co otrzymamy w runtime.

```Typescript
export {}

type Money = number & { readonly type: unique symbol }
declare let m: Money
declare let n: number

m = n // ❌ Type 'number' is not assignable to type 'Money'.
n = m // ✅
```

Używając operatorów matematycznych typ `Money` zostaje sprowadzony do prymitywu `number`, a wynik, który otrzymujemy również jest typu `number`.

```Typescript
m = m + 4 // ❌ Type 'number' is not assignable to type 'Money'
m = m * 4 // ❌ Type 'number' is not assignable to type 'Money'
```

#researchMore Czym jest `{ readonly type: unique symbol }`? 

### Value objects

```Typescript
type Currency = "EUR" | "PLN"

class Money {
  private constructor(
    private value: number,
    private currency: Currency,
  ){}

  // prywatny konstruktor i statyczna metoda fabrykująca
  static from(value: number, currency: Currency){
    return new Money(value, currency)
  }

  // możemy mnożyć tylko przez współczynnik (liczbę)
  multiply(factor: number){
    return new Money(this.value * factor, this.currency)
  }

  // chronimy reguł biznesowych:
  // można dodawać tylko pieniądze w tej samej walucie
  add(another: Money){
    if (this.currency != another.currency){
      throw new Error('Cannot add different currencies')
    }
    return new Money(this.value + another.value, this.currency)
  }

  nominalValue(){
    return this.value
  }
}
```

Co możemy z tym zrobić?

```Typescript
const m = Money.from(99.99, "PLN") // deklaracja

m + 4 // ❌ Operator '+' cannot be applied to types 'Money' and 'number'.

const n: number = m // ❌ is not assignable to type 'number'

const sum1 = m.add( Money.from(1.23, 'PLN') ) // ✅ Money

const sum2 = m.add( Money.from(1.23, 'EUR') ) // ✅ kompiluje się (bo typy są zgodne)
// - ale wybuchnie w runtime

const product = m.multiply( 2 ) // ✅ Money
```

#researchMore W kursie wspomniana jest możliwość stworzenia klasy z generycznym typem określającym Currency, dzięki któremu próba dodania innej waluty za pomocą metody `add` wysypie się już w Runtime

```Typescript
const sum2 = m.add( Money.from(1.23, 'EUR') ) // ❌ weź się wywal!
````

Spróbowałem stworzyć analogiczną klasę, ale nie otrzymałem rządanego rezultatu

```Typescript
class GenericMoney<T extends Currency> {
  private constructor(private value: number, private currency: T) {}

  static from(value: number, currency: Currency) {
    return new GenericMoney<typeof currency>(value, currency);
  }

  multiply(factor: number) {
    return new GenericMoney(this.value * factor, this.currency);
  }

  add(another: GenericMoney<T>) {
    if (this.currency != another.currency) {
      throw new Error("Cannot add different currencies");
    }
    return new GenericMoney(this.value + another.value, this.currency);
  }

  nominalValue() {
    return this.value;
  }
}

const q = GenericMoney.from(10, 'PLN') // q: GenericMoney<Currency>
const sum3 = q.add( GenericMoney.from(1.23, 'EUR') ) // ❌ nadal się kompiluje
````