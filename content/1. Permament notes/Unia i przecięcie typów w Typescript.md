---
date: 2022-04-15
tags: courses/ArchitekturaNaFroncie
---
# Unia i przecięcie typów w Typescript

Typy w Typescript można połączyć na dwa sposoby
1. w unię
2. w przecięcie

```Typescript
interface Human {
  name: string
}

declare let someone: Human

interface Developer extends Human {
  languages: string[]
}
declare let developer: Developer

interface Psychologist extends Human {
  askUncomfortableQuestion(): void
}

declare let psychologist: Psychologist

type DeveloperORPsychologist = Developer | Psychologist
type DeveloperANDPsychologist = Developer & Psychologist
```

Typy określają wymagania jakie stawia zmienna podczas próby przypisania do niej wartości dlatego:

```Typescript
declare let dANDp: DeveloperAndPsychologist
dANDp = developer // ❌
dANDp = psychologist // ❌
```

Zarówno w jednym, jak i drugim przypadku zmienna `developer` i `psychologist` nie spełniają sumy (czyli WSZYSTKICH) wymagań przecięcia typów. Natomiast:

```Typescript
declare let dORp: DeveloperORPsychologist
dORp = developer // ✅
dORp = psychologist // ✅

// ORAZ

dORp = dANDp // ✅
```

Ponieważ wymaganie stawiane przez **unię** to: bądź jednym lub drugim. Oczywiście SUMA również spełnia ten warunek.

---

Próbując zrozumieć unię i przecięcie warto zadawać sobie takie pytania:

> Kto moze należeć i czego na pewno mozemy się spodziewać?

![[unia-intersection.png]]

*--- More*

## Z typem nadrzędnym (nadzbiorem)

![[Pasted image 20220425165607.png]]

Jeżeli unia to suma typów typ `string` i `ABC` sprowadza się do typu `string`.

Przeciwnie w przecięciu, czyli mnożeniu - jeżeli coś ma być jednocześnie stringiem **i** ABC to musi być typu `ABC`