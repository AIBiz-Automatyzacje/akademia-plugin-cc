---
name: zaplanuj
description: Zamienia uzgodniony pomysł albo listę poprawek w plan wykonawczy zapisany w docs/plany/, z sekcją rzeczy do przygotowania przez operatora. Używaj zawsze, gdy użytkownik chce coś zmienić albo dodać w projekcie i jeszcze nie ma planu, np. „zaplanuj te poprawki”, „zrób plan zmiany”, „rozpisz, jak to zbudować”, „co muszę przygotować przed budową”, także gdy wskazuje plik z listą zmian. Ten skill niczego nie buduje (od budowy jest zbuduj) i nie ocenia, czy pomysł ma sens (od tego jest pomysl).
argument-hint: "[plik z listą zmian albo pomysłem, albo opis zmiany]"
---

# Zaplanuj: od uzgodnionej zmiany do planu wykonawczego

Twoim wynikiem jest jeden plik planu. Według niego ktoś inny (skill `zbuduj`, często w nowej sesji i bez tej rozmowy) wykona pracę krok po kroku. Dlatego plan musi być zrozumiały bez dopowiedzeń, a wszystko, czego nie da się zrobić bez człowieka, musi być wypisane z góry. Kodu nie zmieniasz.

## Wejście

Użytkownik podał: $ARGUMENTS

To może być ścieżka do pliku (lista poprawek, pomysł z `docs/pomysly/`), opis zmiany wpisany wprost albo jedno i drugie. Jeśli to ścieżka, przeczytaj ten plik i wpisz go w planie w polu „Źródło”. Jeśli nic nie podano, weź zmianę z rozmowy, a gdy jej tam nie ma, zapytaj, co planujemy.

## 1. Zbierz kontekst

- Przeczytaj `CLAUDE.md`. Jeśli projekt ma `DESIGN.md`, a zmiana dotyczy wyglądu, przeczytaj też ten plik.
- Przeczytaj źródło zmiany: plik wskazany przez użytkownika (np. lista poprawek, pomysł z `docs/pomysly/`) albo opis z rozmowy.
- Obejrzyj w kodzie miejsca, których zmiana dotknie. Plan pisany bez zaglądania do kodu wymienia pliki, których nie ma, i pomija te, które trzeba ruszyć.

## 2. Sprawdź, czy da się to zrobić

Nie oceniasz, czy warto. Sprawdzasz wykonalność i komplet:

- Czy każdy punkt jest na tyle konkretny, że da się napisać „po czym poznamy, że działa”? Jeśli nie, dopytaj.
- Czy punkty sobie nie przeczą i w jakiej kolejności muszą iść (co od czego zależy)?
- Czy coś kosztuje pieniądze, limit albo czas (płatne API, długie przeliczenia)? Takie rzeczy wymagają zgody operatora.
- Czego nie da się zrobić bez człowieka: decyzje, klucze, konta, materiały, potwierdzenia punktów oznaczonych w źródle jako „do potwierdzenia”.

Pytania zadaj razem, w jednej wiadomości, krótko i prostym językiem. Jeśli odpowiedź nie blokuje pisania planu, nie pytaj, tylko wpisz sprawę do sekcji operatora.

## 3. Zapisz plan

Plik: `docs/plany/RRRR-MM-DD-krotka-nazwa.md` (data utworzenia, nazwa małymi literami z myślnikami). Jeśli folderu nie ma, utwórz go. Użyj dokładnie tego układu, bo czytają go skille `zbuduj` i `domknij`:

```markdown
# Plan: <tytuł>

- **Status:** do zrobienia
- **Utworzono:** RRRR-MM-DD
- **Źródło:** <plik albo „rozmowa”>

## Cel
Dwa, trzy zdania prostym językiem: co się zmieni dla użytkownika narzędzia.

## Do przygotowania przez operatora
- [ ] <decyzja, zgoda na koszt, klucz, materiał> – po co to jest i co się stanie, jeśli tego zabraknie

## Kroki
### Krok 1. <nazwa>
- **Co robimy:** …
- **Pliki:** …
- **Po czym poznamy, że działa:** … (coś, co da się sprawdzić testem albo na ekranie)
- [ ] zrobione

## Scenariusze dla testera
1. <co użytkownik klika albo wpisuje> → <co ma zobaczyć>

## Poza zakresem
- <czego świadomie nie robimy w tej zmianie>

## Odstępstwa
(wypełnia skill zbuduj)

## Wykonanie
(wypełnia skill domknij)
```

Zasady treści:

- **Sekcja operatora** ma zawierać wszystko, co trzeba załatwić przed budową. Jeśli nic takiego nie ma, wpisz „Nic do przygotowania.” Budowa zaczyna się dopiero po odhaczeniu tej listy, więc nie wpisuj tu rzeczy, które Claude może zrobić sam.
- **Kroki** mają być małe: jeden krok to jedna zmiana, którą da się sprawdzić osobno. Kolejność ma znaczenie, bo `zbuduj` idzie po kolei.
- Nie dodawaj kroków typu „uruchom testy”, „sprawdź całość testerem” ani „zrób commit”. Testy i tester należą do skilla `zbuduj`, a commit do skilla `domknij`, więc w planie byłyby powtórką.
- **Scenariusze dla testera** pisz z perspektywy użytkownika, od kliknięcia do wyniku. Tester nie zgaduje, więc bez opisanego wyniku scenariusza nie sprawdzi.
- **Poza zakresem** chroni przed dokładaniem rzeczy przy okazji. Wpisz tu też punkty ze źródła, które odkładamy.
- Pisz prostym językiem. Operator nie programuje, ale to on ten plan akceptuje.

## 4. Oddaj wynik

Napisz krótko: ścieżkę do pliku, ile jest kroków, co operator ma przygotować i odhaczyć, oraz że budowę uruchamia skill `zbuduj`. Niczego nie buduj i nie rób commita.
