# Elastyczność Komponentów W React - Kompozycja vs Konfiguracja

![rw-book-cover](https://frontlive.pl/favicons/apple-touch-icon.png)

## Metadata
- Author: [[Olaf Sulich]]
- Full Title: Elastyczność Komponentów W React - Kompozycja vs Konfiguracja
- Category: #articles
- Document Tags: [[shortlist]] 
- URL: https://frontlive.pl/blog/elastyczne-komponenty-react

## Highlights
- Im więcej propsów, tym zmniejsza się czytelność. Wraz z rozwojem takiego komponentu, możemy dojść do wniosku, że czytelniej będzie przekazać cały **obiekt konfiguracyjny**: ([View Highlight](https://read.readwise.io/read/01gq2exytemrqfsdst2fhpyb9e))
- Z drugiej strony, jeśli pracujemy nad wewnętrznym rozwiązaniem, gdzie znamy wszystkie warianty komponentu, może warto rozważyć bardziej sztywne, dopasowane do naszych potrzeb rozwiązanie. Niekoniecznie musi być to w 100% sztywna konfiguracja. ([View Highlight](https://read.readwise.io/read/01gq2f2bjay5dxhzv1httbgt5k))
- Istotne jest to, jak zaczynamy ([View Highlight](https://read.readwise.io/read/01gq2f1wn3a6dxa124faavsvhm))
- Naturalnym podejściem zdaje się rozpoczęcie od przekazania propsów, chleb powszedni React Developera. Jednak nie zawsze jest to dobre rozwiązanie. ([View Highlight](https://read.readwise.io/read/01gq2f1sgsrbxcb7zn9c1jwz18))
- Zaczynając od kompozycji możemy stworzyć niskopoziomowe API komponentu, a następnie nadbudować je sztywną konfiguracją. Niestety nie działa to w drugą stronę. ([View Highlight](https://read.readwise.io/read/01gq2f1pa9g5b16m657dvy715e))
- W obu podejściach musimy pamiętać o jednym - odpowiednim poziomie abstrakcji. Zasada *Don't Repeat Yourself* zakorzeniła się w głowach programistów na stałe. Czy słusznie? Przy tworzeniu komponentów warto mieć z tyłu głowy to, że [duplikacja jest często sporo "tańsza" od niewłaściwej abstrakcji](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction). ([View Highlight](https://read.readwise.io/read/01gq2f1ft8607e2y3dee0sqm1j))
