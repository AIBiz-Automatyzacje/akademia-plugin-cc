---
name: zbuduj
description: Buduje zmianę dokładnie według zapisanego planu z docs/plany/, krok po kroku, z testami po każdym kroku i sprawdzeniem gotowego narzędzia przez subagenta testera. Używaj zawsze, gdy użytkownik chce wykonać gotowy plan, np. „zbuduj to według planu”, „wdrażamy plan”, „zrób poprawki z planu”, „buduj”, „kontynuuj budowę”. Nie używaj, gdy planu jeszcze nie ma (wtedy zaplanuj) ani do samego zapisu zmian w Gicie (wtedy domknij).
argument-hint: "[ścieżka do planu z docs/plany/, opcjonalnie uwagi do budowy]"
---

# Zbuduj: wykonanie planu bez odbiegania od niego

Plan został uzgodniony z operatorem, który nie programuje i nie sprawdzi kodu. Jedyne, na czym może polegać, to że powstało dokładnie to, co jest w planie, i że zostało sprawdzone. Stąd dwie zasady, które rządzą całą pracą: trzymasz się planu i niczego nie uznajesz za gotowe bez dowodu.

## Wejście

Użytkownik podał: $ARGUMENTS

To może być ścieżka do planu i dodatkowe uwagi (np. „zacznij od kroku 6”). Uwagi nie zmieniają planu: jeśli prosi o coś, czego w planie nie ma, potraktuj to jak odstępstwo i postępuj według punktu 4.

## 1. Wczytaj plan i kontekst

- Weź plan wskazany w wejściu. Jeśli użytkownik żadnego nie wskazał, a w `docs/plany/` jest jeden plan ze statusem „do zrobienia”, weź ten. Jeśli jest ich więcej, zapytaj, który.
- Przeczytaj cały plan, `CLAUDE.md` i, przy zmianach wyglądu, `DESIGN.md`. Z `CLAUDE.md` weź polecenie uruchamiające testy i ograniczenia projektu (np. czego nie wolno uruchamiać, bo kosztuje).
- Jeśli część kroków jest już odhaczona, kontynuuj od pierwszego nieodhaczonego.

## 2. Bramka: sekcja operatora

Sprawdź sekcję „Do przygotowania przez operatora”. Jeśli jakikolwiek punkt jest nieodhaczony, zatrzymaj się i wypisz, czego brakuje. Nie zgaduj decyzji i nie zaczynaj „tego, co się da”, bo praca oparta na brakującej decyzji zwykle idzie do kosza. Wyjątek: operator wyraźnie każe zacząć od kroków, które od brakującego punktu nie zależą.

## 3. Buduj krok po kroku

Dla każdego kroku, w kolejności z planu:

1. Zrób tylko to, co opisuje krok.
2. Dopisz albo zaktualizuj test, który sprawdza „po czym poznamy, że działa”. Jeśli tej rzeczy nie da się sprawdzić testem automatycznym (np. wygląd), upewnij się, że pokrywa ją scenariusz dla testera.
3. Uruchom wszystkie testy projektu, nie tylko nowe. Zmiana w jednym miejscu potrafi zepsuć inne.
4. Dopiero przy zielonych testach odhacz krok w pliku planu (`- [x] zrobione`). Plan jest też dziennikiem: po przerwaniu sesji każdy widzi, na czym stanęło.

Nie osłabiaj i nie usuwaj testów po to, żeby przeszły. Jeśli stary test przestał pasować, bo plan zmienia zachowanie, zmień go i wpisz to do „Odstępstw”.

## 4. Trzymaj się planu

- Nie dokładaj niczego, czego w planie nie ma: żadnych ulepszeń przy okazji, porządków w kodzie ani dodatkowych funkcji. Pomysły zapisz na końcu raportu jako propozycje.
- Jeśli kroku nie da się zrobić tak, jak zapisano (błąd w planie, brakujące dane, sprzeczność), zatrzymaj się i zapytaj operatora, podając krótko: co nie pasuje, jakie są wyjścia i które polecasz. Po decyzji wpisz ją do sekcji „Odstępstwa” w planie (krok, co zmieniono, dlaczego) i dopiero wtedy idź dalej.
- Punkty z sekcji „Poza zakresem” są zamknięte.

## 5. Sprawdzenie całości przez testera

Po ostatnim kroku:

1. Uruchom subagenta `tester`. Jeśli projekt ma własnego w `.claude/agents/`, weź tego, bo zna projekt. Jeśli nie, weź testera z tego pluginu. W zadaniu podaj mu ścieżkę do planu i poproś o przejście sekcji „Scenariusze dla testera”. Jeśli żaden z nich nie jest dostępny, uruchom zwykłego subagenta z tym samym zadaniem i zasadą, że nie zmienia kodu, tylko oddaje raport.
2. Błędy z raportu popraw sam, uruchom testy i puść testera ponownie.
3. Po trzech rundach bez czystego raportu przestań i oddaj sprawę operatorowi z listą tego, co nadal nie działa. Dalsze próby zwykle kręcą się w kółko.

Jeśli tester nie mógł wykonać sprawdzenia (np. nie ma dostępu do przeglądarki), to nie jest czysty raport. Zostaw status bez zmian, napisz wprost, co zostało niesprawdzone, i zaproponuj, jak to sprawdzić.

Tester jest osobnym subagentem, bo ten, kto budował, widzi to, co chciał zbudować, a nie to, co jest na ekranie.

## 6. Zakończ

- Jeśli wszystkie kroki są odhaczone i raport testera jest czysty, zmień w planie status na „zbudowane”.
- Nie rób commita ani pusha i nie aktualizuj `CLAUDE.md`. To zadanie skilla `domknij`, żeby operator mógł najpierw sam obejrzeć wynik.
- Oddaj krótki raport prostym językiem: co zrobione, wynik testów, wynik testera, odstępstwa, co operator powinien obejrzeć sam, oraz że zmianę zamyka skill `domknij`.
