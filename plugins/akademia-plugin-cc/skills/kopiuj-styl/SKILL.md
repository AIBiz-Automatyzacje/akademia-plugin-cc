---
name: kopiuj-styl
description: Ogląda wskazaną stronę internetową w przeglądarce tak, jak zrobiłby to człowiek (zrzuty ekranu na komputerze i telefonie plus odczyt prawdziwych kolorów, fontów, zaokrągleń i cieni) i zapisuje w katalogu głównym projektu plik design.md, z którego da się zbudować ekrany aplikacji w tym samym stylu. Używaj zawsze, gdy użytkownik podaje adres strony i chce, żeby jego aplikacja wyglądała podobnie, np. „skopiuj styl z …”, „zrób design.md na podstawie tej strony”, „chcę taki wygląd jak na …”, „weź kolory i czcionki z …”, „/kopiuj-styl https://…”. Nie buduje ekranów (od tego jest zbuduj) i nie przenosi z cudzej strony tekstów, zdjęć ani logo.
argument-hint: "[adres strony, np. https://example.com]"
---

# Kopiuj styl: wygląd cudzej strony w pliku design.md

Użytkownik zna stronę, która mu się podoba, i chce, żeby jego aplikacja wyglądała podobnie. Twoim wynikiem jest jeden plik: `design.md` w katalogu głównym projektu. Następne polecenie użytkownika zwykle brzmi „Wygląd weź z pliku @design.md”, więc ten plik jest jedynym, co zobaczy osoba budująca ekrany. Musi wystarczyć do zbudowania przycisków, kart, formularzy i całych ekranów w React + Tailwind bez zaglądania na tamtą stronę.

Dwie rzeczy decydują o jakości pliku. Wartości mają być prawdziwe: odczytane z wyliczonych stylów strony, nie zgadnięte z obrazka (oko myli `#2563EB` z `#3B6FE0`, a fonty myli prawie zawsze). Charakter ma być zobaczony: z samego kodu nie widać, że strona jest gęsta, plakatowa albo spokojna, a to ten opis sprawia, że nowe ekrany „czują się” jak oryginał.

## Wejście

Użytkownik podał: $ARGUMENTS

To powinien być adres strony. Jeśli brakuje `https://`, dopisz go. Jeśli adresu nie ma ani tutaj, ani w rozmowie, zadaj jedno pytanie: „Podaj adres strony, której wygląd mam skopiować.” Przy kilku adresach zapytaj, z której strony brać styl.

## 1. Zanim otworzysz stronę

- Katalog główny projektu to katalog, w którym pracuje ta sesja. Plik zapisujesz właśnie tam, nie w `docs/` ani w podfolderze.
- Sprawdź, czy w katalogu głównym jest już `design.md` (także `DESIGN.md`). Jeśli jest, zapytaj przed jakąkolwiek pracą: „W projekcie jest już plik design.md. Nadpisać go stylem z <adres>?” Na „nie” niczego nie zapisujesz i kończysz. Pytasz na starcie, bo oglądanie strony trwa, a odmowa na końcu wyrzuciłaby całą pracę.

## 2. Otwórz stronę w przeglądarce

W aplikacji desktopowej Claude masz wbudowaną przeglądarkę, narzędzia `mcp__Claude_Browser__*`. Użyj ich tak samo jak agent `tester` z tego pluginu:

- `tabs_create`, potem `navigate` z adresem strony,
- `resize_window` z `width: 1440, height: 900` (komputer) albo `width: 390, height: 844` (telefon), po zmianie rozmiaru przeładuj stronę przez `navigate`,
- `computer` z `action: "screenshot"` robi zrzut, `action: "hover"` najeżdża kursorem, `action: "key"` naciska klawisz (np. `Tab`),
- `javascript_tool` z `action: "javascript_exec"` uruchamia skrypty odczytu z punktu 4 i przewija stronę.

Przewijaj przez `javascript_tool` poleceniem `window.scrollTo({ top: 1350, behavior: "instant" })`. Bez `behavior: "instant"` strony z płynnym przewijaniem potrafią nie ruszyć albo zawiesić polecenie.

`javascript_tool` służy tylko do czytania i przewijania. Możesz dopisywać własne krótkie odczyty (styl konkretnego elementu, reguły z arkuszy stylów), ale niczego nim nie klikasz, nie wpisujesz i nie zmieniasz w wyglądzie strony.

Jeśli wbudowanej przeglądarki nie ma, a masz inne narzędzie do przeglądarki (np. Claude in Chrome), zrób to samo nim. Jeśli nie masz żadnego, przejdź do punktu 5.

Po otwarciu poczekaj, aż strona się dociągnie (2–3 sekundy, animacje wejścia muszą się skończyć). Baner cookies zamknij przyciskiem odrzucenia albo zamknięcia („Odrzuć”, „Tylko niezbędne”, „×”), nigdy nie akceptujesz zgód w imieniu użytkownika. Baner nie jest częścią stylu, nie opisuj go jako komponentu. Jeśli strona pokazuje logowanie, captchę albo błąd, powiedz to wprost jednym zdaniem i poproś o inny adres albo o zrzuty ekranu.

## 3. Obejrzyj stronę jak człowiek

Zrób co najmniej sześć zrzutów i każdy obejrzyj:

1. **Komputer (1440 px):** góra strony, potem dwa dalsze ekrany po przewinięciu (mniej więcej 1,5 i 3 wysokości ekranu w dół). Na długiej stronie dorzuć ekran przy stopce.
2. **Telefon (390 px):** góra strony i dwa dalsze ekrany.

Po każdym zrzucie zanotuj sobie, co widać:

- **Nastrój i gęstość:** spokojnie czy tłoczno, dużo powietrza czy upakowane, poważnie czy zadziornie.
- **Powtarzalne motywy:** twarde cienie, grube obramowania, gradienty, siatka w tle, pigułki, tekst o stałej szerokości znaków, zaznaczenia markerem, ikony liniowe albo wypełnione.
- **Przyciski i karty:** kształt, wypełnienie czy obrys, cień, jak odróżnia się przycisk główny od drugorzędnego.
- **Rozkład kolorów:** który kolor zajmuje najwięcej miejsca, a który pojawia się tylko punktowo jako akcent. Kolor z małej powierzchni łatwo przecenić, z dużego tła łatwo przeoczyć.
- **Telefon:** co się zmienia (wielkość nagłówków, układ kolumn, menu).

Zrzuty zostają w rozmowie, do projektu ich nie zapisujesz.

## 4. Odczytaj prawdziwe wartości

Przewiń stronę raz do końca (żeby dociągnęły się sekcje ładowane przy przewijaniu), wróć na górę i uruchom poniższy skrypt przez `javascript_tool`. Uruchom go dwa razy: w widoku komputera i w widoku telefonu. Skrypt tylko czyta wyliczone style i zwraca zestawienie. Wklej go w całości, bez zmian:

```js
(() => {
  const cx = document.createElement('canvas').getContext('2d', { willReadFrequently: true }), pamiec = {};
  const hex = (c) => {
    if (!c || c === 'transparent' || c === 'none') return null;
    if (c in pamiec) return pamiec[c];
    cx.clearRect(0, 0, 1, 1); cx.fillStyle = '#000'; cx.fillStyle = c; cx.fillRect(0, 0, 1, 1);
    const [r, g, b, a] = cx.getImageData(0, 0, 1, 1).data;
    const h = '#' + [r, g, b].map((v) => v.toString(16).padStart(2, '0')).join('') + (a < 255 ? a.toString(16).padStart(2, '0') : '');
    return (pamiec[c] = a === 0 ? null : h);
  };
  const lum = (h) => {
    const v = [1, 3, 5].map((i) => parseInt(h.slice(i, i + 2), 16) / 255)
      .map((c) => (c <= 0.03928 ? c / 12.92 : ((c + 0.055) / 1.055) ** 2.4));
    return 0.2126 * v[0] + 0.7152 * v[1] + 0.0722 * v[2];
  };
  const kontrast = (a, b) => {
    if (!a || !b || a.length < 7 || b.length < 7) return null;
    const [x, y] = [lum(a.slice(0, 7)), lum(b.slice(0, 7))].sort((p, q) => q - p);
    return Math.round(((x + 0.05) / (y + 0.05)) * 100) / 100;
  };
  const nalozona = (k, pod) => {
    if (!k || k.length !== 9 || !pod) return k;
    const a = parseInt(k.slice(7, 9), 16) / 255;
    return '#' + [1, 3, 5].map((i) => Math.round(parseInt(k.slice(i, i + 2), 16) * a + parseInt(pod.slice(i, i + 2), 16) * (1 - a)).toString(16).padStart(2, '0')).join('');
  };
  const tloPod = (el) => {
    for (let e = el; e; e = e.parentElement) {
      const b = hex(getComputedStyle(e).backgroundColor);
      if (b && b.length === 7) return b;
    }
    return hex(getComputedStyle(document.documentElement).backgroundColor) || '#ffffff';
  };
  const policz = (mapa, klucz, waga = 1) => { if (klucz) mapa[klucz] = (mapa[klucz] || 0) + waga; };
  const top = (mapa, n = 10) => Object.entries(mapa).sort((a, b) => b[1] - a[1]).slice(0, n).map(([k, v]) => [k, Math.round(v)]);
  const widoczny = (el) => { const r = el.getBoundingClientRect(); return r.width > 2 && r.height > 2 && getComputedStyle(el).visibility !== 'hidden'; };
  const vw = innerWidth;
  const tla = {}, teksty = {}, akcenty = {}, fonty = {}, wagi = {}, promienie = {}, cienie = {}, obrysy = {}, gradienty = {}, odstepy = {}, sekcje = {}, szerokosci = {}, ruch = {};
  const elementy = [...document.body.querySelectorAll('*')].filter(widoczny).slice(0, 4000);
  for (const el of elementy) {
    const s = getComputedStyle(el), r = el.getBoundingClientRect();
    const pole = Math.min(r.width * r.height, vw * 900) / 1000;
    policz(tla, hex(s.backgroundColor), pole);
    if (s.backgroundImage.includes('gradient')) policz(gradienty, s.backgroundImage.slice(0, 300), 1);
    const wlasnyTekst = [...el.childNodes].filter((n) => n.nodeType === 3).map((n) => n.textContent.trim()).join('').length;
    if (wlasnyTekst) {
      policz(teksty, hex(s.color), wlasnyTekst);
      policz(fonty, s.fontFamily.split(',')[0].replace(/["']/g, '').trim(), wlasnyTekst);
      policz(wagi, s.fontWeight, wlasnyTekst);
    }
    if (s.borderRadius !== '0px' && r.width > 20) policz(promienie, s.borderRadius, 1);
    if (s.boxShadow !== 'none') policz(cienie, s.boxShadow.slice(0, 140), 1);
    if (parseFloat(s.borderTopWidth) > 0 && s.borderTopStyle !== 'none') policz(obrysy, s.borderTopWidth + ' ' + hex(s.borderTopColor), 1);
    if (['flex', 'grid', 'inline-flex'].includes(s.display) && s.gap !== 'normal' && s.gap !== '0px') policz(odstepy, s.gap, 1);
    if (r.width >= vw * 0.95 && r.height > 200 && (parseFloat(s.paddingTop) > 0 || parseFloat(s.paddingBottom) > 0)) policz(sekcje, s.paddingTop + ' / ' + s.paddingBottom, 1);
    if (s.transitionDuration !== '0s') policz(ruch, s.transitionDuration.split(',')[0] + ' ' + s.transitionTimingFunction.split(/,(?![^(]*\))/)[0], 1);
    if (s.maxWidth !== 'none' && s.maxWidth.endsWith('px') && r.width > 300) policz(szerokosci, s.maxWidth, 1);
  }
  const unikalne = (lista, n) => { const w = {}; for (const o of lista) { const k = JSON.stringify(o); w[k] = (w[k] || 0) + 1; } return Object.entries(w).sort((a, b) => b[1] - a[1]).slice(0, n).map(([k, v]) => ({ ile: v, ...JSON.parse(k) })); };
  const tloStrony = tloPod(document.body);
  const naglowki = ['h1', 'h2', 'h3', 'h4'].map((t) => {
    const el = [...document.querySelectorAll(t)].find(widoczny);
    if (!el) return null;
    const nosnik = [el, ...el.querySelectorAll('*')].map((e) => [e, [...e.childNodes].filter((n) => n.nodeType === 3).map((n) => n.textContent.trim()).join('').length]).sort((a, b) => b[1] - a[1])[0][0];
    const sn = getComputedStyle(nosnik);
    return { tag: t, font: sn.fontFamily, rozmiar: sn.fontSize, waga: sn.fontWeight, interlinia: sn.lineHeight, odstepLiter: sn.letterSpacing, wielkieLitery: sn.textTransform, kolor: hex(sn.color) || 'przezroczysty (tekst z gradientem albo animacją, kolor sprawdź na zrzucie)', gradientTekstu: sn.backgroundImage.includes('gradient') ? sn.backgroundImage.slice(0, 120) : undefined, tlo: tloPod(el) };
  }).filter(Boolean);
  const duzeTeksty = unikalne(elementy.filter((el) => parseFloat(getComputedStyle(el).fontSize) >= 28 && [...el.childNodes].some((n) => n.nodeType === 3 && n.textContent.trim().length > 1)).map((el) => {
    const s = getComputedStyle(el);
    return { font: s.fontFamily.split(',')[0], rozmiar: s.fontSize, waga: s.fontWeight, interlinia: s.lineHeight, odstepLiter: s.letterSpacing, wielkieLitery: s.textTransform, kolor: hex(s.color) };
  }), 50).sort((a, b) => parseFloat(b.rozmiar) - parseFloat(a.rozmiar)).slice(0, 6);
  const style = {};
  for (const p of [...document.querySelectorAll('p, li')].filter(widoczny)) {
    const s = getComputedStyle(p), d = p.textContent.trim().length;
    if (d < 40) continue;
    const k = JSON.stringify({ font: s.fontFamily.split(',')[0], rozmiar: s.fontSize, waga: s.fontWeight, interlinia: s.lineHeight, kolor: hex(s.color), tlo: tloPod(p) });
    style[k] = (style[k] || 0) + d;
  }
  const akapity = top(style, 3).map(([k, v]) => { const o = JSON.parse(k); return { znakow: v, ...o, kontrast: kontrast(o.kolor, o.tlo) }; });
  const opisz = (el) => {
    const s = getComputedStyle(el), r = el.getBoundingClientRect(), tlo = nalozona(hex(s.backgroundColor), tloPod(el.parentElement || el)) || tloPod(el);
    return { tlo: hex(s.backgroundColor) || 'przezroczyste', gradient: s.backgroundImage.includes('gradient') ? s.backgroundImage.slice(0, 300) : undefined, tekst: hex(s.color), kontrast: kontrast(hex(s.color), tlo), font: s.fontFamily.split(',')[0], rozmiar: s.fontSize, waga: s.fontWeight, wielkieLitery: s.textTransform !== 'none' ? s.textTransform : undefined, promien: s.borderRadius, padding: s.padding, wysokosc: Math.round(r.height), obrys: parseFloat(s.borderTopWidth) > 0 ? s.borderTopWidth + ' ' + hex(s.borderTopColor) : undefined, cien: s.boxShadow !== 'none' ? s.boxShadow.slice(0, 120) : undefined };
  };
  const przyciski = unikalne([...document.querySelectorAll('button, a, [role=button], input[type=submit]')].filter(widoczny).filter((el) => {
    const s = getComputedStyle(el), r = el.getBoundingClientRect();
    return r.height >= 28 && r.height <= 90 && r.width < 520 && (hex(s.backgroundColor) || s.backgroundImage.includes('gradient') || parseFloat(s.borderTopWidth) > 0);
  }).map(opisz), 6);
  przyciski.forEach((p) => { if (p.tlo !== 'przezroczyste') policz(akcenty, p.tlo, p.ile); });
  const karty = unikalne(elementy.filter((el) => {
    const s = getComputedStyle(el), r = el.getBoundingClientRect();
    return r.width > 160 && r.width < vw * (vw < 600 ? 0.99 : 0.9) && r.height > 100 && parseFloat(s.borderRadius) > 0 && parseFloat(s.paddingTop) + parseFloat(s.paddingLeft) > 0 && el.textContent.trim().length > 10 && (s.boxShadow !== 'none' || parseFloat(s.borderTopWidth) > 0 || (hex(s.backgroundColor) && hex(s.backgroundColor) !== tloPod(el.parentElement)));
  }).map((el) => { const o = opisz(el); delete o.wysokosc; delete o.rozmiar; delete o.waga; delete o.font; delete o.kontrast; delete o.tekst; delete o.wielkieLitery; return o; }), 5);
  const pola = unikalne([...document.querySelectorAll('input:not([type=hidden]):not([type=submit]):not([type=checkbox]):not([type=radio]), textarea, select')].filter(widoczny).map((el) => {
    const o = opisz(el);
    if (!o.obrys && o.tlo === 'przezroczyste') {
      for (let e = el.parentElement, i = 0; e && i < 3; e = e.parentElement, i++) {
        const r = opisz(e);
        if (r.obrys || r.tlo !== 'przezroczyste') { o.ramkaNaRodzicu = { tlo: r.tlo, obrys: r.obrys, promien: r.promien, padding: r.padding, wysokosc: r.wysokosc, cien: r.cien }; break; }
      }
    }
    return o;
  }), 3);
  const etykiety = unikalne(elementy.filter((el) => {
    const s = getComputedStyle(el), r = el.getBoundingClientRect();
    return r.height < 44 && r.width < 320 && el.textContent.trim().length > 1 && el.textContent.trim().length < 45 && el.children.length <= 2 && parseFloat(s.borderRadius) > 0 && (hex(s.backgroundColor) || parseFloat(s.borderTopWidth) > 0) && !el.closest('button, a, mark, h1, h2, h3');
  }).map(opisz), 4);
  const zmienne = {};
  for (const arkusz of document.styleSheets) {
    let reguly; try { reguly = arkusz.cssRules; } catch (e) { continue; }
    for (const r of reguly || []) {
      if (r.selectorText && /^(:root|html|body)$/.test(r.selectorText.trim())) {
        for (const p of r.style) if (p.startsWith('--') && Object.keys(zmienne).length < 80) {
          const v = r.style.getPropertyValue(p).trim();
          if (/#|rgb|hsl|oklch|px|rem|font|shadow|radius/i.test(v + p)) zmienne[p] = v.slice(0, 80);
        }
      }
    }
  }
  if (!Object.keys(zmienne).length) {
    const sr = getComputedStyle(document.documentElement);
    for (const p of sr) if (p.startsWith('--') && Object.keys(zmienne).length < 80) {
      const v = sr.getPropertyValue(p).trim();
      if (/^(#|rgb|hsl|oklch)/i.test(v) || /font|shadow|radius/i.test(p)) zmienne[p] = v.slice(0, 80);
    }
  }
  const stany = [];
  for (const arkusz of document.styleSheets) {
    let reguly; try { reguly = arkusz.cssRules; } catch (e) { continue; }
    for (const r of reguly || []) {
      if (stany.length >= 20 || !r.selectorText || !/:(hover|focus-visible|focus)\b/.test(r.selectorText)) continue;
      const w = {}; for (const p of ['background-color', 'background', 'color', 'border-color', 'box-shadow', 'transform', 'outline', 'outline-offset', 'text-decoration']) { const v = r.style.getPropertyValue(p); if (v) w[p] = v.slice(0, 90); }
      if (Object.keys(w).length) stany.push({ selektor: r.selectorText.slice(0, 80), ...w });
    }
  }
  const wczytaneFonty = {};
  document.fonts.forEach((f) => { if (f.status === 'loaded') policz(wczytaneFonty, f.family.replace(/["']/g, '') + ' ' + f.weight, 1); });
  const zrodlaFontow = [...document.querySelectorAll('link[href*="font"], link[href*="typekit"]')].map((l) => l.href.slice(0, 140));
  return {
    adres: location.href, szerokoscOkna: vw, tloStrony, schemat: getComputedStyle(document.documentElement).colorScheme,
    naglowki, akapity,
    tla: top(tla, 8), kolorytekstu: top(teksty, 8), kolorPrzyciskow: top(akcenty, 5), gradienty: top(gradienty, 4),
    fonty: top(fonty, 6), wagi: top(wagi, 6), wczytaneFonty: Object.keys(wczytaneFonty).slice(0, 15), zrodlaFontow,
    promienie: top(promienie, 8), cienie: top(cienie, 6), obrysy: top(obrysy, 6),
    odstepyMiedzyElementami: top(odstepy, 8), paddingSekcji: top(sekcje, 5), maksSzerokoscTresci: top(szerokosci, 5), przejscia: top(ruch, 4),
    duzeTeksty, przyciski, karty, pola, etykiety, stanyZArkuszy: stany, zmienneCss: zmienne,
  };
})()
```

Jak czytać wynik:

- **`zmienneCss`**: jeśli strona ma nazwane zmienne kolorów (np. `--akcent`, `--brand-primary`), to najpewniejsza paleta. Weź z nich role i wartości.
- **`tla`, `kolorytekstu`, `kolorPrzyciskow`**: kolory posortowane według tego, ile ich jest (tła ważone powierzchnią, teksty liczbą znaków). Liczby są przybliżone, bo zagnieżdżone elementy sumują się kilka razy, więc rolę koloru potwierdzaj na zrzutach.
- **`naglowki`, `duzeTeksty`, `akapity`**: `naglowki` to pierwsze h1–h4 na stronie, `duzeTeksty` to wszystkie teksty od 28 px w górę, od największego (tu zwykle jest prawdziwy napis z góry strony, nawet gdy siedzi w zagnieżdżonym elemencie), `akapity` to najczęstsze style zwykłego tekstu. Porównaj wynik z komputera i z telefonu, z tego bierzesz skalę nagłówków dla obu ekranów.
- **`fonty`, `wczytaneFonty`, `zrodlaFontow`**: font z adresem `fonts.googleapis.com` albo `fonts.gstatic.com` jest darmowy. Każdy inny sprawdź, otwierając `https://fonts.googleapis.com/css2?family=Nazwa+Kroju` (spacje zamień na `+`): odpowiedź ze stylami znaczy, że krój jest w Google Fonts, błąd znaczy, że go tam nie ma. Krój spoza Google Fonts (np. `sohne-var`, `Circular`, `Graphik`, pliki z Adobe) zamień na darmowy z Google Fonts o podobnym charakterze (szerokość, geometria, grubości) i zapisz oba: co jest na stronie i czego używać w projekcie. Skrypt podaje grubość zadeklarowaną w stylach, a nie narysowaną: jeśli krój ma tylko jedną grubość (np. Archivo Black ma tylko 400), w pliku wpisz tę, którą krój naprawdę ma.
- **`przyciski`, `karty`, `pola`, `etykiety`**: najczęstsze style tych elementów z kolorami, zaokrągleniem, paddingiem, obrysem i cieniem. Pole `kontrast` to wyliczony stosunek kontrastu tekstu do tła. `ramkaNaRodzicu` przy polu znaczy, że obrys i zaokrąglenie ma element wokół pola, a nie samo pole.
- **`stanyZArkuszy`**: reguły po najechaniu i przy zaznaczeniu z klawiatury (fokus), jeśli arkusze stylów są dostępne. Na niektórych stronach będą puste.
- **`promienie`, `cienie`, `obrysy`, `odstepyMiedzyElementami`, `paddingSekcji`, `maksSzerokoscTresci`, `przejscia`**: najczęstsze wartości. Z nich budujesz skale.

Wynik skryptu jest prawdą o liczbach, zrzuty są prawdą o charakterze i proporcjach. Gdy jedno przeczy drugiemu, nie wpisuj żadnej z wersji na ślepo, tylko sprawdź element dokładniej własnym odczytem. Typowe przypadki: nagłówek wygląda na granatowy, a skrypt podaje zielony (napis ma nałożone warstwy animacji, weź kolor warstwy, którą widać); skrypt pokazuje białe tło, a na zrzucie dominuje niebieski pas (tło jest obrazkiem albo gradientem).

Doczytaj osobno to, czego skrypt nie złapał, a co trzeba opisać w komponentach:

- **Po najechaniu:** najedź kursorem na przycisk (`computer`, `action: "hover"`), odczekaj sekundę (kolory często zmieniają się płynnie) i odczytaj jego styl: `(() => { const el = [...document.querySelectorAll(':hover')].pop(); const s = getComputedStyle(el.closest('a, button') || el); return [s.backgroundColor, s.color, s.boxShadow, s.transform, s.borderColor]; })()`.
- **Fokus:** naciśnij kilka razy `Tab` (`computer`, `action: "key"`), aż zaznaczy się przycisk albo link, i odczytaj `(() => { const s = getComputedStyle(document.activeElement); return [s.outline, s.outlineOffset, s.boxShadow]; })()`.
- **Pola formularza ukryte w okienku albo nieobecne:** weź ich wygląd z `stanyZArkuszy` albo z reguł w arkuszach stylów. Gdy strona nie ma żadnych pól, zaprojektuj pole z wartości strony (tło, obrys, promień, font) i oznacz je jako propozycję.

## 5. Zapas: gdy nie ma przeglądarki

Bez żadnego narzędzia do przeglądarki nie zrobisz zrzutów ani nie odczytasz wyliczonych stylów. Jeśli użytkownik nie wkleił zrzutu ekranu tej strony, napisz mu na początku i poczekaj na odpowiedź: „Nie mam tu przeglądarki, więc pracuję bez zrzutów ekranu: kolory i czcionki odczytam z kodu strony. Jeśli wkleisz zrzut ekranu tej strony (wystarczy jej góra), opiszę też jej charakter. Możesz też napisać «bez zrzutu».”

Potem:

1. Pobierz stronę zwykłym narzędziem do pobierania stron. Gdy masz terminal, użyj `curl -sL <adres>`, bo daje dosłowny kod; WebFetch przy dużych plikach potrafi oddać streszczenie zamiast wartości. Wypisz z niej adresy arkuszy stylów (`<link rel="stylesheet">`) i style wpisane w `<style>`.
2. Pobierz arkusze stylów (główny to zwykle ten ze zmiennymi w `:root` i stylami `body`, nie koniecznie największy). Jeśli zmienne w `:root` powtarzają się w kilku blokach `@media`, wartości na komputer bierz z bloku dla największej szerokości. Szukaj w nich zmiennych w `:root`, kolorów (`#…`, `rgb(…)`, `oklch(…)`), `font-family` i `@font-face`, `border-radius`, `box-shadow`, `border`, reguł `:hover` i `:focus-visible`. Przy WebFetch proś o dosłowne wartości z kodu, nie o streszczenie.
3. Rolę koloru ustalaj po tym, gdzie stoi w kodzie (`body`, `h1`, `.btn`, `.card`), a nie po tym, jak często występuje.
4. Jeśli użytkownik wkleił zrzut, opisz z niego charakter tak jak w punkcie 3. Jeśli nie, pisz opis ostrożnie i tylko z tego, co wynika z kodu.

W pliku, w sekcji `## Overview`, dopisz zdanie: „Plik powstał bez oglądania strony w przeglądarce: wartości pochodzą z kodu strony, opis charakteru jest przybliżony.” W komunikacie końcowym powiedz to samo prostymi słowami.

## 6. Kontrast

Wolno odwzorować wygląd, ale tekst musi być czytelny. Sprawdź kontrast par, które trafią do pliku: tekst główny i pomocniczy na każdym tle, tekst przycisków na ich tle, tekst etykiet, obrys pól na tle. Progi: zwykły tekst co najmniej 4,5:1, duży tekst (od 24 px albo od 19 px pogrubiony), obrysy pól formularza i obwódka fokusu na każdym tle co najmniej 3:1.

Nie licz kontrastu z głowy. Skrypt podaje go dla akapitów, przycisków, pól i etykiet; dla pozostałych par uruchom w przeglądarce (`javascript_tool`) tę funkcję z parami kolorów:

```js
((pary) => pary.map(([a, b]) => { const l = (h) => { const v = [1, 3, 5].map((i) => parseInt(h.slice(i, i + 2), 16) / 255).map((c) => (c <= 0.03928 ? c / 12.92 : ((c + 0.055) / 1.055) ** 2.4)); return 0.2126 * v[0] + 0.7152 * v[1] + 0.0722 * v[2]; }; const [x, y] = [l(a), l(b)].sort((p, q) => q - p); return a + ' na ' + b + ': ' + ((x + 0.05) / (y + 0.05)).toFixed(2); }))([['#888888', '#ffffff'], ['#ffffff', '#3366ff']])
```

Bez przeglądarki policz ten sam wzór poleceniem `node -e` albo `python3 -c`, jeśli są dostępne; jeśli nie ma żadnego z nich, zaznacz w pliku, że kontrastu nie sprawdzono.

Gdy para nie spełnia progu, zmień ją tak, żeby wygląd ucierpiał jak najmniej. Zwykle przyciemniasz albo rozjaśniasz kolor tekstu w tym samym odcieniu. Gdy to by zmieniło charakter (biały napis na średnio jasnym przycisku musiałby zrobić się czarny), przyciemnij lekko tło przycisku i zostaw biały napis. W pliku zapisz obie wartości jednym zdaniem, np. „Tekst pomocniczy: na stronie `#A0A4AB` (2,5:1), tu podniesiony do `#6E737B` (4,8:1), żeby był czytelny.”

## 7. Granice: co wolno wziąć, a czego nie

Wolno odwzorować blisko oryginału: kolory i sposób ich użycia, typografię (skalę, grubości, rytm), zaokrąglenia, cienie, obrysy, odstępy, układ i gęstość, sposób budowania przycisków i kart oraz ruch (przejścia, animacje).

Nie wolno przenosić: tekstów ze strony (nagłówków, haseł, opisów, nazw kategorii, nawet jako przykładów w pliku), zdjęć, ilustracji i ikon jako plików, logo, nazwy firmy ani niczego, co ją identyfikuje. W `name` wpisz opisową nazwę stylu („Ciepły papier”, „Spokojny fintech”), nie nazwę firmy. Adres strony możesz podać raz w `## Overview` jako źródło inspiracji. Przykłady w opisie komponentów pisz neutralne („Zapisz”, „Dalej”, „Nazwa projektu”). Ikony i grafiki opisz słowami (styl, grubość linii, kolory), bez linków do plików ze strony.

## 8. Zapisz design.md

Format jest zgodny ze specyfikacją Google Labs design.md (https://github.com/google-labs-code/design.md): na górze tokeny w YAML między liniami `---`, niżej opis w sekcjach `##` w stałej kolejności. Nagłówki sekcji zostają po angielsku, bo tak je rozpoznają narzędzia czytające ten format. Treść piszesz po polsku.

Zasady wartości:

- Kolory jako hex w cudzysłowie (`"#2563EB"`). Kolor z przezroczystością przelicz na 8-znakowy hex (`rgba(20, 20, 20, 0.07)` to `"#14141412"`).
- Każda wartość w pliku pochodzi z odczytu (punkt 4 albo 5) albo jest jawnie oznaczona: „(podniesione dla kontrastu)”, „(z arkusza stylów, na stronie niewidoczne)” lub „(propozycja, strona nie ma tego elementu)”. Niczego nie dopisujesz „dla kompletu” bez oznaczenia.
- Rozmiary w `px`: wartości z ułamkiem zaokrąglij do pełnych pikseli (56,16 px to 56 px), odstęp liter do 0,1 px. Nagłówki w tokenach podaj w rozmiarze na komputer; rozmiary na telefon zapisz w sekcji `## Typography`.
- `fontFamily` w tokenach to font do użycia w projekcie (darmowy). Font ze strony, jeśli inny, podaj w opisie.
- Role kolorów: nazwy tokenów opisują rolę, nie odcień (`primary`, `on-primary`, `surface`, `on-surface-muted`), bo osoba budująca ekrany szuka „koloru tekstu pomocniczego”, nie „szarego 2”.
- Szablon to minimum. Dopisz tokeny i komponenty, które budują charakter tej strony (drugi równorzędny przycisk główny, karta wyróżniona, zakreślenie markerem). Format przyjmuje dodatkowe nazwy. Gdy strona ma dwa równorzędne przyciski główne, opisz w `## Components`, kiedy używać którego.

Szablon (zamień wartości na odczytane, zbędne tokeny usuń, brakujące role dopisz):

````markdown
---
version: alpha
name: <opisowa nazwa stylu>
description: <jedno zdanie o charakterze>
colors:
  primary: "#…"          # główny akcent: przyciski główne, zaznaczenia
  on-primary: "#…"       # tekst na primary
  secondary: "#…"        # drugi kolor marki, jeśli jest
  neutral: "#…"          # tło strony
  surface: "#…"          # tło kart i paneli
  surface-alt: "#…"      # tło sekcji wyróżnionych, jeśli są
  on-surface: "#…"       # tekst główny
  on-surface-muted: "#…" # tekst pomocniczy
  border: "#…"           # obrysy i linie
  success: "#…"          # tylko jeśli strona tego używa
  error: "#…"            # gdy strona nie pokazuje, propozycja
typography:
  display:
    fontFamily: …
    fontSize: …px
    fontWeight: …
    lineHeight: …
    letterSpacing: …px
  headline-lg: …         # ten sam układ pól
  headline-md: …
  headline-sm: …
  body-lg: …
  body-md: …
  body-sm: …
  label-md: …            # przyciski, etykiety
  mono: …                # tylko jeśli strona używa kroju o stałej szerokości
rounded:
  none: 0px
  sm: …px
  md: …px
  lg: …px
  full: 9999px
spacing:
  xs: …px
  sm: …px
  md: …px
  lg: …px
  xl: …px
  section: …px           # odstęp między sekcjami strony
  container: …px         # maksymalna szerokość treści
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.full}"
    padding: …px …px
    height: …px
    borderWidth: …px     # tylko gdy przycisk ma obrys
    borderColor: "{colors.border}"
    shadow: "…"          # dosłownie w CSS, np. "0 2px 8px #0000001A"
  button-primary-hover:
    backgroundColor: "#…"
    shadow: "…"
  button-secondary: …
  card: …
  input: …
  input-focus: …
  label: …               # etykieta, pigułka, znaczek
---

## Overview
3–5 zdań: nastrój, gęstość, co rzuca się w oczy jako pierwsze, jakie motywy się powtarzają. Źródło inspiracji: <adres>.

## Colors
Każdy kolor z rolą i miejscem użycia: „**Primary (#…):** …”. Napisz, który kolor zajmuje dużo miejsca, a który tylko punktowo. Tu też zdania o podniesionym kontraście.

## Typography
Font na stronie i font w projekcie (z linkiem do Google Fonts i grubościami do wczytania). Skala nagłówków na komputer i telefon. Odstęp liter, wielkie litery, gdzie krój o stałej szerokości.

## Layout
Maksymalna szerokość treści, odstępy między sekcjami, typowe odstępy w kartach i listach, liczba kolumn na komputerze i telefonie, gęstość.

## Elevation & Depth
Cienie dosłownie w CSS (np. `0 2px 8px #0000001A`), gdzie który. Jeśli strona nie używa cieni, napisz, czym buduje głębię (obrysy, tła).

## Shapes
Zaokrąglenia (co jest pigułką, co ma mały promień), obrysy (grubość i kolor), linie podziału.

## Motion & Micro-interaction
Czasy i krzywe przejść ze strony, co się dzieje po najechaniu i kliknięciu. Przy braku danych: krótkie, spokojne przejścia, oznaczone jako propozycja.

## Components
Przycisk główny i drugorzędny, karta, pole formularza, etykieta: wygląd w stanie zwykłym, po najechaniu i przy fokusie, z wartościami. Plus inne powtarzalne elementy, które budują charakter (np. pasek z przewijającym się tekstem, panel w stylu terminala).

## Do's and Don'ts
- Rób: 4–6 punktów, które utrzymują charakter.
- Nie rób: 4–6 punktów, które by go zepsuły. Złe przykłady opisuj słowami („fioletowe gradienty”), bez kodów kolorów.

## Tailwind
Gotowy blok do wklejenia do głównego pliku CSS (Tailwind 4); w starszej wersji te same wartości idą do `tailwind.config`. Wystarczą kolory, fonty, promienie i cienie, plus tło strony, jeśli to wzór (kratka, siatka); skala nagłówków na telefon zostaje w `## Typography`.

```css
@import url("https://fonts.googleapis.com/css2?family=…&display=swap");
@theme {
  --color-primary: #…;
  --font-sans: "…", system-ui, sans-serif;
  --radius-card: …px;
  --shadow-card: …;
}
```
````

## 9. Sprawdź plik, zanim oddasz

Przeczytaj gotowy `design.md` jeszcze raz i sprawdź:

- Każdy hex z pliku występuje w odczycie albo ma oznaczenie „podniesione dla kontrastu” lub „propozycja”.
- Fonty w tokenach są darmowe, a link do Google Fonts zawiera właściwe grubości.
- Są wszystkie sekcje z szablonu, w tej kolejności, i komponenty: przycisk główny, drugorzędny, karta, pole, etykieta.
- W pliku nie ma żadnego zdania ani nazwy przepisanej ze strony, nazwy firmy w `name`, logo ani linków do obrazków ze strony.
- YAML na górze jest poprawny: kolory w cudzysłowach, odwołania w formie `"{colors.primary}"`, cienie w cudzysłowach.

## 10. Powiedz, co zrobione

Na koniec 3–5 linii prostym językiem, bez słów „token”, „hex”, „YAML”, „CSS”:

1. Co zapisałeś i gdzie: „Zapisałem plik design.md w głównym folderze projektu.”
2. Jednym zdaniem charakter stylu (np. „Jasne tło, granatowy tekst, zielone zaokrąglone przyciski i miękkie cienie”).
3. Co zmieniłeś względem oryginału, jeśli coś: zamiennik czcionki, podniesiony kontrast, praca bez zrzutów.
4. Jak użyć: „W następnym poleceniu napisz np.: Zbuduj ekran logowania. Wygląd weź z pliku @design.md”.
