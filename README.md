# akademia-plugin-cc: od pomysłu do gotowej zmiany

Plugin do Claude Code z kursu „Zbuduj aplikację w Claude Code” (Akademia Automatyzacji). Cztery skille, które prowadzą zmianę w Twoim narzędziu od pomysłu do zapisu na GitHubie, skill, który kopiuje wygląd wskazanej strony, i agent, który sprawdza wynik w przeglądarce.

| Skill | Co robi | Co zostawia |
|---|---|---|
| `/akademia-plugin-cc:pomysl` | Przegaduje pomysł, podważa go i wydaje werdykt: robimy, odkładamy albo odrzucamy | plik w `docs/pomysly/` |
| `/akademia-plugin-cc:zaplanuj` | Zamienia uzgodnioną zmianę w plan z krokami i listą rzeczy, które musisz przygotować | plik w `docs/plany/` |
| `/akademia-plugin-cc:zbuduj` | Buduje według planu, krok po kroku, z testami i sprawdzeniem przez testera | odhaczone kroki w planie |
| `/akademia-plugin-cc:domknij` | Aktualizuje `CLAUDE.md`, przenosi plan do `docs/archiwum/`, robi commit i push | plan w archiwum z datą wykonania |
| `/akademia-plugin-cc:kopiuj-styl` | Ogląda wskazaną stronę w przeglądarce i spisuje jej wygląd | plik `design.md` w głównym folderze projektu |

Po nazwie skilla podajesz plik albo opis, np. `/akademia-plugin-cc:zaplanuj docs/poprawki.md`.

## Kopiuj styl

Podajesz adres strony, która Ci się podoba, a skill otwiera ją w przeglądarce wbudowanej w aplikację desktopową Claude. Robi zrzuty ekranu w widoku komputera i telefonu, odczytuje prawdziwe kolory, czcionki, zaokrąglenia i cienie, po czym zapisuje je w pliku `design.md` w głównym folderze projektu.

```
/akademia-plugin-cc:kopiuj-styl https://example.com
```

W następnym poleceniu wystarczy napisać na przykład „Zbuduj ekran logowania. Wygląd weź z pliku @design.md”. Skill bierze z cudzej strony tylko wygląd. Teksty, zdjęcia i logo zostają tam, gdzie były. Płatną czcionkę zamienia na darmową z Google Fonts, a za słabo widoczny tekst przyciemnia albo rozjaśnia, żeby dało się go przeczytać. Bez wbudowanej przeglądarki (na przykład w terminalu) skill odczyta kolory i czcionki z kodu strony i uprzedzi, że pracuje bez zrzutów.

## Instalacja

### W aplikacji desktopowej Claude

1. Otwórz **Customize**, potem **Plugins**.
2. Kliknij **Add**, a następnie **Add marketplace**.
3. Wpisz adres `AIBiz-Automatyzacje/akademia-plugin-cc` i zatwierdź.
4. Na liście pluginów zainstaluj `akademia-plugin-cc`.

### W terminalu

W sesji Claude Code wpisz po kolei:

```
/plugin marketplace add AIBiz-Automatyzacje/akademia-plugin-cc
/plugin install akademia-plugin-cc@akademia-automatyzacji
```

## Czego potrzebuje projekt

- pliku `CLAUDE.md` z opisem projektu i poleceniem uruchamiającym testy,
- repozytorium Git połączonego z GitHubem (skill `domknij` robi commit i push),
- folderu `docs/` (skille same założą w nim `pomysly`, `plany` i `archiwum`).

Agent `tester` korzysta z przeglądarki wbudowanej w aplikację desktopową Claude.
