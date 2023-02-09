---
date: 2022-09-03
tags: []
published: true
---
# Uruchamianie aplikacji w dni robocze

Mam nadzieję, że będzie to idealny wpis na sobotę.

Czy zdarza się Wam tak, że odpalacie w weekend komputer, którego używacie również do pracy i po zalogowaniu, wyłączacie niepotrzebne aplikacje, np. Slacka? Ja tak miałem i zamarzyła mi się opcja, by niektóre aplikacje uruchamiały się przy starcie wyłącznie w określone dni tygodnia. 

Niestety, spośród tych, których używam, żadna aplikacja nie posiada takiej funkcji. Dlatego stworzyłem ją sam, a dzisiaj pokażę Wam, jak to zrobić, krok po kroku. Będziesz potrzebował [AutoHotkey](https://www.autohotkey.com/), który niejednokrotnie był już polecany na Ahoy.

## Wyłącz automatyczne uruchamianie w aplikacji

Przede wszystkim potrzebujesz wyłączyć automatyczne uruchamianie aplikacji w jej ustawieniach. Zazwyczaj nie potrzeba za wiele szukać. We wspomnianym już Slacku ta opcja znajduje się tutaj:

![[Pasted image 20220903104921.png]]

Jeżeli Twoja aplikacja nie posiada takiej opcji w ustawieniach, znajdź ją w ustawieniach **Startup apps** Windowsa.

![[Pasted image 20220903105100.png]]

## Stwórz nowy skrypt AutoHotkey

Jeśli zainstalowałeś już [AutoHotkey](https://www.autohotkey.com/) na swoim komputerze, utwórz w dowolnym katalogu plik z rozszerzeniem `.ahk`. Następnie otwórz go w notatniku.

Zasada proponowanego przeze mnie rozwiązania mieści się w pojedynczej instrukcji warunkowej: `if`. Za jej pomocą sprawdzamy, czy dzisiaj jest dzień roboczy. Ja dodatkowo sprawdzam, czy jest przed 18. Służą do tego dwie zmienne wbudowane w AutoHotkey: `A_WDay` oraz `A_Hour`. Niedziela jest oznaczona cyfrą `0` niezależnie od ustawień lokalizacji systemu, dlatego potrzebujemy podwójnego sprawdzenia. Tak samo godzina, niezależnie od systemu, zwracana jest w postaci dwucyfrowej 24-godzinnego zegara (od 0 do 23).

U mnie wygląda to następująco:

```
if (A_WDay > 1 and A_WDay < 7 and A_Hour < 18) {
  Run, slack
  Run, "C:\Program Files\Docker\Docker\Docker Desktop.exe"
}
```

Wewnątrz instrukcji warunkowej odpalam konkretne aplikacje poleceniem `Run`. Niektóre programy (tak jak slack) rejestrują się w PATH, dlatego do ich odpalenia wystarczy krótkie hasło `slack`. Inne jak np. Docker tego nie robią, dlatego do ich uruchomienia potrzebujemy odnaleźć ścieżke do pliku `.exe`.  Możecie to zrobić w ten sposób:

1. Znajdź program w menu start
2. Kliknij prawym przyciskiem myszy
3. Z menu wybierz More(Więcej) > Open file location (Otwórz lokalizację pliku)
4. Znowu kliknij prawym przyciskiem myszy na ikonę w nowootwartym folderze i wybierz Properties (Właściwości)
5. W oknie właściwości znajdź pole Target, jego wartość skopiuj i wklej jako argument polecenia `Run,` (po przecinku, jak na przykładzie).

>[!important]
>Ważne: Jeśli w ścieżce do pliku `.exe` znajdują się spacje, musisz całą tę ściężkę zamknąć w cudzysłowach.

![[vEPrmjHk4k.png]]

## Już prawie koniec: dodaj skrypt do katalogu Startup

Żeby Twój skrypt uruchamiał się zawsze po uruchomieniu komputera, musisz dodać go do odpowiedniego katalogu, w którym znajdują się skróty do programów, które mają się uruchamiać podczas startu. Ścieżka do tego folderu znajduje się tutaj:

```
C:\Users\[NAZWA TWOJEGO UŻYTKOWNIKA]\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

`[NAZWA TWOJEGO UŻYTKOWNIKA]` znajduje się w katalogu `C:\Users`. Gdy będziesz już znać pełną ścieżkę możesz otworzyć ją z łatwością wklejając ją do okna polecenia (które otworzysz skrótem klawiaturowym `Win+R`). 

Skoro już znajdziesz się w tym sekretnym folderze, wróć na chwilę do folderu ze skryptem a następnie skopiuj plik. Potem w folderze Startup kliknij prawym przyciskiem myszy i wybierz Paste shortcut (Wklej skrót). *I voilà!*