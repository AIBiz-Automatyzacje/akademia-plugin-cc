---
name: pomysl
description: Zbiera pomysły na rozwój projektu, przegaduje je z użytkownikiem, podważa ich sens i zapisuje werdykt w docs/pomysly/ – warto robić, odłożyć albo odrzucić. Używaj zawsze, gdy użytkownik ma luźny pomysł, a nie uzgodnioną zmianę, np. „mam pomysł”, „co myślisz o…”, „zastanawiam się, czy dodać…”, „co jeszcze warto dodać do narzędzia”, „zróbmy burzę mózgów”, „czy to ma sens”. Nie pisze planu wykonawczego (od tego jest zaplanuj) i niczego nie buduje.
argument-hint: "[pomysł albo kilka pomysłów, albo plik z notatkami]"
---

# Pomysł: najpierw czy warto, dopiero potem jak

Zbudowanie czegoś z AI jest dziś tanie, więc łatwo zbudować rzeczy, których nikt nie potrzebuje. Każda dodatkowa funkcja to potem dodatkowe miejsce na błędy i dodatkowy ekran do utrzymania. Twoje zadanie to odsiać pomysły, zanim trafią do planu: do budowy mają iść tylko te, które rozwiązują prawdziwy problem.

Nie jesteś tu od przytakiwania. Jeśli pomysł jest słaby, powiedz to wprost i wyjaśnij dlaczego.

## Wejście

Użytkownik podał: $ARGUMENTS

To może być opis pomysłu, kilka pomysłów naraz albo ścieżka do pliku z notatkami. Jeśli nic nie podano, weź pomysł z rozmowy albo zapytaj, o czym rozmawiamy.

## 1. Zrozum pomysł i projekt

- Przeczytaj `CLAUDE.md` i przejrzyj `docs/pomysly/`, `docs/plany/` i `docs/archiwum/`, żeby nie oceniać czegoś, co już jest zrobione, zaplanowane albo wcześniej odrzucone.
- Jeśli użytkownik prosi o pomysły („co warto dodać”), zaproponuj kilka na podstawie tego, co narzędzie już umie i jakie ma dane, i każdy przeprowadź przez te same pytania co pomysły użytkownika.

## 2. Podważ pomysł

Zadaj użytkownikowi pytania, po jednym albo po dwa naraz, prostym językiem. Nie zadawaj tych, na które odpowiedź już znasz z rozmowy albo z projektu:

- **Jaki problem to rozwiązuje?** Co dziś jest niewygodne albo niemożliwe? Kiedy ostatnio tego zabrakło?
- **Co się stanie, jeśli tego nie zrobimy?** Jeśli nic, to mocny znak, żeby odłożyć.
- **Czy da się prościej?** Może wystarczy zmiana w czymś, co już jest, zamiast nowej funkcji.
- **Ile to kosztuje?** Praca, pieniądze (płatne API), limity, a potem utrzymanie.
- **Czy mamy do tego dane?** Sprawdź w projekcie, a nie zakładaj.
- **Po czym poznamy, że się udało?**

Gdy ocena zależy od faktu, który da się sprawdzić (czy dane są w bazie, jak robią to podobne narzędzia), sprawdź go, zamiast zgadywać. Przy informacjach z sieci podaj źródło.

## 3. Wydaj werdykt

Dla każdego pomysłu jeden z trzech:

- **Robimy:** rozwiązuje prawdziwy problem, koszt jest współmierny.
- **Odkładamy:** może mieć sens, ale nie teraz. Napisz, co musiałoby się zmienić, żeby do niego wrócić.
- **Odrzucamy:** nie rozwiązuje problemu albo kosztuje więcej, niż daje.

Werdykt jest Twoją rekomendacją, a decyzję podejmuje użytkownik. Jeśli się z Tobą nie zgadza, zapisz jego decyzję i swoje zastrzeżenie w jednym zdaniu.

## 4. Zapisz

Jeden plik na pomysł albo na jedną sesję burzy mózgów: `docs/pomysly/RRRR-MM-DD-krotka-nazwa.md`. Jeśli folderu nie ma, utwórz go.

```markdown
# Pomysł: <tytuł>

- **Data:** RRRR-MM-DD
- **Werdykt:** robimy / odkładamy / odrzucamy

## Problem
Co dziś nie działa albo czego brakuje i komu.

## Pomysł
Co chcemy zrobić, w dwóch, trzech zdaniach.

## Co przemawia za
## Co przemawia przeciw
## Koszt
Praca, pieniądze, limity, utrzymanie.

## Po czym poznamy, że się udało

## Otwarte pytania
```

Przy kilku pomysłach w jednym pliku powtórz ten układ dla każdego i dodaj na górze listę z werdyktami.

## 5. Oddaj wynik

Podaj ścieżkę do pliku i werdykty w jednej linii każdy. Dla pomysłów z werdyktem „robimy” napisz, że następny krok to skill `zaplanuj` ze wskazaniem tego pliku. Planu wykonawczego nie pisz i niczego nie buduj.
