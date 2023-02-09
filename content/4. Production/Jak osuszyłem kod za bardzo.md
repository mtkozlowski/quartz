---
date: 2022-08-19
tags: []
published: true
---
# Jak osuszyłem kod za bardzo

1. Wprowadzenie
	1. o aplikacji
	2. o tym, że chciałem się wykazać
	3. o chęci wykorzystania dotychczasowo zdobytej wiedzy i nauczenia się jeszcze więcej
2. Dlaczego tak bardzo nie chciałem siebie powtarzać?
	1. Ponieważ kilkadziesiąt bardzo podobnych tabel wydawało mi się oczywistem kandydatem dla tego antywzorca
	2. Ponieważ zadeklarowanie jednej właściwości dwa razy: jako kolumny w tabeli i formularza (tworzenia i edycji) to było dla mnie marnowanie czasu i powtarzanie się.
	3. Ponieważ w poprzednim projekcie, do którego dołączyłem na końcu, wszędzie lał się zduplikowany kod.
3. Do czego to doprowadziło?
	1. Rozwiązanie stało się bardzo hermetyczne
	2. Trudno rozbudowywać funkcję budującą (Open - Closed)
	3. W ten sposób można uzyskać jedynie bardzo płaski formularz, który jest jedynie długą lub jeszcze dłuższą listą pól do wypełnienia. Trudno podzielić go na sekcje, dodać śródtytuły lub cokolwiek innego, co ułatwiłoby użytkownikowi przetrawienie formularza.
4. Wnioski
	1. Zbyt pochopnie uznałem dwa różne komponenty (tabelę oraz formularz) nawet w tej ilości za podobne i wymagające "uproszczenia" i automatyzacji.
	2. Każdy z tych elementów to inny scenariusz i zasługuje na indywidualne podejście, spójne wewnątrz design systemu, który staram się wypracować dla tej aplikacji
	3. Czas, który poświęciłem na przygotowanie tego sprytnego rozwiązania był pewnie porównywalny z przygotowaniem każdego widoku z osobna.
	4. Dla uporząkowania kodu mogę zastosować partiale

## Wprowadzenie

Jakiś czas temu zostałem przydzielony do pracy nad aplikacją dla nowego klienta. Projekt dotyczy aplikacji webowej do zarządzania firmą szkoleniową i jej wszystkimi podmiotami: placówkami, trenerami, uczestnikami, itd. Potraktowałem to zlecenie jako okazję do wykazania się. Postanowiłem jak najlepiej wykorzystać dotychczasowo zdobytą wiedzę i nauczyć się jeszcze więcej.

Jednym z pierwszych zadań było utworzenie widoków dla ciągle rosnącej liczby bytów. Każdy z nich powinien mieć tabelę i formularz pozwalający na tworzenie lub edycję wpisu. Nie pamiętam, jak wiele widoków było do stworzenia na samym początku, ale w tej chwili w jest ich ok. 100.

Bardzo szybko zniechęciłem się do tego zadania, ponieważ wydawało się bardzo żmudne, powtarzalne i nigdy się niekończące. A terminy były na wczoraj. Wtedy właśnie w mojej głowie pojawiła się myśl, że trzeba to jakoś uogólnić, ugenerycznić, skompresować, żeby przestać się tak bardzo powtarzać.

Idea niepowtarzania się podziałała na mnie jak czerwona płachta na byka, ponieważ w poprzednim projekcie, do którego dołączyłem w końcowej fazie utrzymania były kilogramy powielonego kodu. Czułem, że muszę przegnać to przekleństwo z mojego życia.

## One Eternity later
![[Pasted image 20220819112451.png]]

Nie jestem w stanie policzyć wszystkich godzin, które spędziłem na pracy nad tym szalonym pomysłem. Ostatecznie powstał `formBuilder`, który spełnia swoją funkcję. Jest to funkcja, która mapuje tablicę obiektów zawierających konfigurację pól formularza. Dzięki generycznemu typowaniu udało mi się sprawić, że intelisens podpowiada dostępne pola danych oraz rodzaje inputów.

Zatem udało mi się osiągnąć mój cel. Teraz przy użyciu jednej listy byłem w stanie wyświetlić i tabelę, i formularz! Wow. Niestety pojawienie się komplikacji było tylko kwestią czasu. Moje rozwiązanie, choć byłem z niego bardzo dumny, było bardzo hermetyczne. Nie tylko nie spełniało zasady Open-Close (i pewnie kilka innych, ale mniejsza o spełnianie zasad co do litery). Co gorsza, rezultatem była jedynie długa, płaska lista pól, która oferuje kiepską czytelność i użyteczność użytkownikowi.

## Wracając po rozum do głowy

Okres wakacji i chwilowa zmiana priorytetów w firmie sprawiły, że w zeszłym tygodniu wróciłem do tematu z odświeżoną głową, za którą szybko się złapałem widząc swoje dotychczasowe dokonania. Na szczęście, dzięki odpoczynkowi odzyskałem też serce do tej aplikacji.

Na szczęście projekt wyszedł już z fazy, w której klientowi i szefostwu zależy na jak najszybszych widocznych rezultatach. To również presja czasu sprawiła, że wpadłem w pułapkę jak największego uproszczenia. W tej chwili mogę z zaangażowaniem pracować nad widokami, które jak najlepiej posłużą wszystkim pracownikom firmy klienta.

Zdałem sobie sprawę, że zbyt pochopnie uznałem dwa różne komponenty (tabelę oraz formularz) - nawet w tej ilości - za podobne i wymagające "uproszczenia" i automatyzacji. Każdy z tych elementów to inny scenariusz dla użytkownika i zasługuje na indywidualne podejście, spójne wewnątrz design systemu, który staram się wypracować dla tej aplikacji.

Czas, który poświęciłem na przygotowanie tego sprytnego rozwiązania był pewnie porównywalny z przygotowaniem każdego widoku z osobna. Na dobre wyjdzie mi na pewno to, ile nowych rzeczy nauczyłem się o Typescript & React. Projekt na szczęście bardzo na tym nie ucierpiał. Obok istniejących już tabel będą dodawane kolejne elementy i wówczas stopniowo zastąpię mojego dzielnego `formBuildera` dedykowanymi zestawami komponentów.

**A co Wam zdarzyło się przedobrzyć w pracy?**