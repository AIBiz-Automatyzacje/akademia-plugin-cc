---
name: domknij
description: Zamyka zbudowaną zmianę – aktualizuje CLAUDE.md, dopisuje do planu, co zostało wykonane, przenosi plan do docs/archiwum/ z datą wykonania, oznacza zrobione punkty na liście źródłowej i robi commit oraz push. Używaj zawsze po zakończonej budowie, gdy użytkownik pisze np. „domknij zmianę”, „zamknij ten plan”, „zapisz wszystko i wyślij na GitHuba”, „posprzątaj po budowie”, „zarchiwizuj plan”. Nie buduje i nie uruchamia testów (to robi zbuduj).
argument-hint: "[ścieżka do planu z docs/plany/, opcjonalnie uwagi]"
---

# Domknij: porządek po zbudowanej zmianie

Po budowie zostają trzy luźne końce: `CLAUDE.md` opisuje projekt sprzed zmiany, plan leży wśród planów do zrobienia, a praca istnieje tylko na tym komputerze. Zamykasz wszystkie trzy, żeby za miesiąc dało się odczytać, co zrobiono i kiedy, a następna sesja zaczynała z aktualnym opisem projektu.

## Wejście

Użytkownik podał: $ARGUMENTS

To może być ścieżka do planu i uwagi do zamknięcia (np. „krok 3 odkładamy”). Uwagi zapisz w sekcji „Wykonanie”.

## 1. Znajdź plan

- Weź plan wskazany przez użytkownika albo jedyny plan w `docs/plany/` ze statusem „zbudowane”.
- Rzuć okiem na zmiany (`git diff`) i porównaj je z odhaczonymi krokami. Nie chodzi o ponowne testowanie, tylko o to, żeby w archiwum nie wylądował plan, który mówi „zrobione” o czymś, czego w kodzie nie ma.
- Krok nieodhaczony, ale wyjaśniony w sekcji „Odstępstwa” (np. odłożony decyzją operatora), nie wymaga pytania. Opisz go w „Wykonaniu” jako niezrobiony.
- Jeśli status jest inny (np. „do zrobienia”), są nieodhaczone kroki bez wyjaśnienia albo odhaczony krok nie ma pokrycia w zmianach, nie zamykaj od razu. Napisz, co jest niedokończone, i zapytaj, czy mimo to domykać. Zamknięcie niedokończonej zmiany jest dozwolone, ale ma być świadomą decyzją i ma zostać opisane w planie.
- Jeśli użytkownik w wejściu sam wyjaśnił brak (np. „krok 3 zostaje otwarty”) i kazał domykać, to jest ta świadoma decyzja. Nie pytaj drugi raz, tylko opisz to w „Wykonaniu”.
- Testów nie uruchamiasz. Sprawdzanie to zadanie budowy, a tutaj polegasz na statusie planu.

## 2. Zaktualizuj CLAUDE.md

Porównaj zmiany (`git status`, `git diff`) z treścią `CLAUDE.md` i popraw tylko to, co przestało być prawdą albo czego brakuje: polecenia, budowa projektu, ograniczenia, ścieżki do plików. Nie przepisuj pliku od nowa i nie wklejaj do niego historii zmiany, bo ta zostaje w planie. `CLAUDE.md` czytany jest na starcie każdej sesji, więc ma być krótki (do ok. 200 linii).

Jeśli zmiana nie wpływa na nic, co opisuje `CLAUDE.md`, zostaw go bez zmian i napisz to w raporcie.

## 3. Dopisz do planu sekcję „Wykonanie”

```markdown
## Wykonanie
- **Zakończono:** RRRR-MM-DD
- **Zrobione:** krótko, co powstało (prostym językiem)
- **Odstępstwa od planu:** skrót z sekcji „Odstępstwa” albo „brak”
- **Niezrobione:** kroki pominięte i dlaczego, albo „brak”
- **Do sprawdzenia ręcznie:** co operator powinien sam obejrzeć
```

Jeśli plan ma już sekcję „Wykonanie” z notatkami z budowy, zostaw je i dopisz te pola pod spodem.

Zmień status na górze planu na „zrobione”.

## 4. Przenieś plan do archiwum

Przenieś plik do `docs/archiwum/` poleceniem `git mv` (zachowuje historię pliku) i dodaj na początku nazwy datę wykonania:

`docs/plany/2026-11-18-poprawki-panelu.md` → `docs/archiwum/2026-11-21-wykonano-poprawki-panelu.md`

Jeśli folderu nie ma, utwórz go. Po tym kroku w `docs/plany/` zostają wyłącznie plany, które czekają na budowę.

## 5. Oznacz punkty na liście źródłowej

Jeśli plan ma w polu „Źródło” plik (np. listę poprawek albo pomysł), zaznacz w nim zrobione punkty: dopisz przy nich „✅ zrobione RRRR-MM-DD, plan: <ścieżka w archiwum>”. Punktów niezrobionych nie ruszaj. Treści punktów nie kasuj.

## 6. Commit i push

- Sprawdź `git status`. Do commita mają wejść: zmiany z budowy, `CLAUDE.md`, przeniesiony plan i lista źródłowa. Nie dodawaj plików z kluczami ani danych (np. `.env`, folder z bazą). Jeśli widzisz w zmianach coś, co nie należy do tej zmiany, zapytaj, zanim to dodasz.
- Zrób jeden commit z opisem prostym językiem: co się zmieniło dla użytkownika narzędzia, plus ścieżka do planu w archiwum.
- Wyślij na GitHuba (`git push`). Jeśli push się nie uda, nie próbuj na siłę (żadnego `--force`), tylko podaj dosłowny komunikat.

## 7. Raport

Krótko, prostym językiem: co dopisano do `CLAUDE.md`, gdzie jest plan w archiwum, które punkty listy źródłowej są oznaczone, czy commit i push przeszły, oraz co operator ma sprawdzić ręcznie.
