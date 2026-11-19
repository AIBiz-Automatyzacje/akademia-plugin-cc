# akademia-plugin-cc: od pomysłu do gotowej zmiany

Plugin do Claude Code z kursu „Zbuduj aplikację w Claude Code” (Akademia Automatyzacji). Cztery skille, które prowadzą zmianę w Twoim narzędziu od pomysłu do zapisu na GitHubie, i agent, który sprawdza wynik w przeglądarce.

| Skill | Co robi | Co zostawia |
|---|---|---|
| `/akademia-plugin-cc:pomysl` | Przegaduje pomysł, podważa go i wydaje werdykt: robimy, odkładamy albo odrzucamy | plik w `docs/pomysly/` |
| `/akademia-plugin-cc:zaplanuj` | Zamienia uzgodnioną zmianę w plan z krokami i listą rzeczy, które musisz przygotować | plik w `docs/plany/` |
| `/akademia-plugin-cc:zbuduj` | Buduje według planu, krok po kroku, z testami i sprawdzeniem przez testera | odhaczone kroki w planie |
| `/akademia-plugin-cc:domknij` | Aktualizuje `CLAUDE.md`, przenosi plan do `docs/archiwum/`, robi commit i push | plan w archiwum z datą wykonania |

Po nazwie skilla podajesz plik albo opis, np. `/akademia-plugin-cc:zaplanuj docs/poprawki.md`.

## Instalacja

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
