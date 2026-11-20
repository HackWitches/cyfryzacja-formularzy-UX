---
name: dostepnosc
description: Audyt dostępności cyfrowej serwisu publicznego według wytycznych gov.pl („Jakie elementy tworzą dostępność cyfrową”, 27 elementów), WCAG 2.1 AA i ustawy o dostępności cyfrowej. Działa na kodzie (HTML, React, CSS), adresie URL, zrzucie ekranu, pliku Figma i tekstach (prosty język). Zwraca priorytetową listę problemów z elementem gov.pl, numerem kryterium WCAG i konkretną poprawką, a na prośbę wprowadza bezpieczne poprawki. Używaj przy pytaniach o dostępność, accessibility, a11y, WCAG, kontrast, czytniki ekranu, klawiaturę, prosty język, deklarację dostępności lub przed oddaniem ekranu czy demo.
argument-hint: <plik | folder | URL | link do Figmy> [--fix]
---

# Audyt dostępności (serwis publiczny)

Piszesz po polsku, krótko, listami i tabelami. Audytujesz cel z `$ARGUMENTS`. Jeśli argument jest pusty, zapytaj jednym zdaniem, co sprawdzić.

Źródło checklisty: https://www.gov.pl/web/dostepnosc-cyfrowa/jakie-elementy-tworza-dostepnosc-cyfrowa i jej podstrony (`https://www.gov.pl/web/dostepnosc-cyfrowa/<slug>`). Numery kryteriów WCAG przypisaliśmy sami, bo gov.pl ich nie podaje.

## Kontekst prawny

- **Ustawa z 4 kwietnia 2019 r. o dostępności cyfrowej stron internetowych i aplikacji mobilnych podmiotów publicznych** wymaga poziomu **WCAG 2.1 AA** (norma EN 301 549).
- **Deklaracja dostępności** jest obowiązkowa dla każdej strony i aplikacji podmiotu publicznego (art. 10). Wymagania:
  - publikuje się ją na stronie lub w aplikacji, której dotyczy,
  - **link do niej jest na każdej podstronie** (stopka lub nagłówek),
  - sama deklaracja musi być w pełni dostępna, nawet jeśli serwis nie jest,
  - ma identyfikatory `id` (`a11y-wstep`, `a11y-podmiot`, `a11y-email` i inne, pełna lista w warunkach technicznych BIP),
  - dobra praktyka: `<meta name="deklaracja-dostępności" content="<URL deklaracji>">`,
  - przy aplikacji mobilnej publikuje się ją na stronie podmiotu i w aplikacji, a link umieszcza w sklepie z aplikacjami.
- Kryteria **WCAG 2.2** (np. 2.4.11, 2.5.8, 3.3.7, 3.3.8) nie są jeszcze obowiązkiem. Traktuj je jako rekomendacje i oznaczaj osobno.
- Transmisje na żywo nie podlegają ustawie. Nagranie, które zostaje na stronie po transmisji, już tak.

## Zasady audytu

- **Nigdy nie pisz „zgodne z WCAG”** na podstawie samych testów automatycznych. Wypisz, czego nie sprawdzono.
- Każdy problem ma: **element gov.pl**, kryterium WCAG (numer i poziom A/AA), miejsce (plik:linia, selektor lub warstwa), **kogo dotyczy** i konkretną poprawkę.
- Priorytety: 🔴 blokuje wykonanie zadania · 🟠 utrudnia · 🟢 drobne lub rekomendacja (WCAG 2.2/AAA, dobra praktyka gov.pl).
- Z `--fix` wprowadzaj tylko bezpieczne poprawki: atrybuty, etykiety, semantykę, kontrast w tokenach, teksty. Przed zmianą układu lub logiki zapytaj.
- Najpierw sprawdzaj tokeny i komponenty design systemu. Jedna poprawka naprawia wtedy wszystkie ekrany.

## Tryby

- **Kod:** przejrzyj komponenty i style, uruchom testy automatyczne (jeśli aplikacja startuje), przejdź checklistę.
- **URL:** `npx @axe-core/cli <url>`, `npx pa11y <url>`, walidator W3C, a potem checklista.
- **Projekt (zrzut, Figma):** kontrast, rozmiary celów, etykiety, stany (focus, błąd, disabled), komunikaty, kolejność czytania, informacje przekazywane samym kolorem, kształtem lub pozycją. Wypisz, co dopisać w specyfikacji dla developerów: kolejność fokusu, alt, role i nazwy, obszary, `lang`.
- **Tekst (etykiety, komunikaty, treści):** sekcja „Prosty język”.

## Checklista: 27 elementów gov.pl → WCAG 2.1

### A. Struktura i semantyka

**Nagłówki** (`naglowki`) · 1.3.1 A, 2.4.6 AA
- Nagłówki tworzone znacznikami `h1–h6`, a nie samym pogrubieniem czy rozmiarem.
- Hierarchia bez przeskoków poziomów (po `h1` nie ma `h3`). gov.pl nazywa przeskok błędem.
- Treść nagłówka informuje, czego dotyczy sekcja.

**Listy elementów** (`listy-elementow`) · 1.3.1 A
- Listy w `ul`/`ol`. Nie udawaj listy myślnikami, cyframi, spacjami ani tabulatorem.
- `ol` stosuj, gdy kolejność ma znaczenie lub liczba elementów jest zamknięta, a `ul` w pozostałych przypadkach.
- Menu jako lista.

**Tabele** (`tabele`) · 1.3.1 A
- Tabele służą tylko do danych, nigdy do układu strony.
- Tabela ma `<th>` (z `scope`), a złożone relacje opisuje przez `headers`/`id`.
- Ogranicz scalanie komórek. Nie zagnieżdżaj tabel i unikaj pustych komórek. Złożoną tabelę podziel na prostsze.

**Istotne obszary** (`istotne-obszary`) · 1.3.1 A, 2.4.1 A
- Landmarki: `header`, `nav`, `main` (jeden), `search` lub `role="search"`, `footer`, a przy kilku `nav` także `aria-label`.
- Układ zgodny z konwencją: nawigacja u góry, wyszukiwarka zwykle w prawym górnym rogu, treść, stopka.

**Tytuły** (`tytuly`) · 2.4.2 A
- Unikalny `<title>` opisujący podstronę lub krok, np. „Krok 2 z 5: Adres – Wniosek o … – mObywatel”. Czytnik czyta go jako pierwszy.

**Język treści** (`jezyk-tresci`) · 3.1.1 A, 3.1.2 AA
- `<html lang="pl">`, a fragmenty w innym języku mają własny `lang`.

**Poprawność kodu** (`poprawnosc-kodu`) · 4.1.1 A (WCAG 2.1)
- Kod przechodzi walidator W3C (https://validator.w3.org): bez błędnych zagnieżdżeń i zduplikowanych `id`. Poprawny wygląd nie oznacza poprawnego kodu.

**ARIA** (`aria`) · 4.1.2 A
- Najpierw natywny HTML, ARIA tylko gdy HTML nie wystarcza (np. stan pola). Błędne ARIA szkodzi.
- `aria-label` nie zastępuje widocznej etykiety, bo osoba sterująca głosem nie zna niewidocznej nazwy (2.5.3 A).
- Wzorce komponentów według W3C ARIA APG.

### B. Percepcja

**Kontrast kolorów** (`kontrast-kolorow`) · 1.4.3 AA, 1.4.11 AA
- Tekst **4,5:1**. Duży tekst (od 24 px, a pogrubiony od ok. 18,7 px) **3:1**.
- Istotne elementy graficzne (ramka pola, ikona-przycisk, słupek wykresu, wskaźnik fokusu) **3:1**.
- Przy spełnionych minimach osobna wersja wysokokontrastowa nie jest potrzebna.

**Cechy wizualne** (`cechy-wizualne`) · 1.4.1 A, 1.3.3 A
- Kolor nigdy nie jest jedynym nośnikiem informacji: błąd to kolor + ikona + tekst, link w tekście to kolor + podkreślenie, wykres to kolor + deseń lub typ linii albo opis.
- Instrukcje nie odwołują się do koloru, kształtu ani pozycji („kliknij czerwony przycisk”, „kółko po lewej”). Zamiast tego: użyj przycisku „Odrzuć” w panelu „Filtrowanie”.

**Elementy graficzne** (`elementy-graficzne`) · 1.1.1 A, 1.4.5 AA
- `alt` zwięźle mówi, co grafika przedstawia w tym kontekście. Pisz w czasie teraźniejszym i stronie czynnej. Nie zaczynaj od „Grafika przedstawiająca…”.
- `alt` to nie podpis ani `title`.
- Ozdobniki: `alt=""` (albo `aria-hidden="true"` dla SVG).
- Link lub przycisk graficzny: `alt` mówi, **dokąd prowadzi lub co robi** („Wróć do poprzedniego kroku”), a nie jak wygląda („Strzałka w lewo”).
- Wykres: obok dane w dostępnej tabeli. Infografika: rozbudowany opis obok lub link do niego.
- Bez tekstu w formie obrazu (wyjątek: logo, które musi mieć `alt`).

**Powiększanie tekstu** (`powiekszanie-tekstu`) · 1.4.4 AA, 1.4.10 AA, 1.4.12 AA
- Nie blokuj zoomu (`user-scalable=no`, `maximum-scale=1` to błąd).
- Przy 200% i przy zwiększonych odstępach nic nie znika ani nie nachodzi na siebie.
- Przy szerokości 320 px (zoom 400%) nie ma przewijania w poziomie. Zwinięcie menu do przycisku jest w porządku.
- Własne przyciski A-/A/A+ nie są potrzebne, bo ograniczają użytkownika.

**Multimedia** (`multimedia`) · 1.2.1–1.2.5 A/AA
- Opis tekstowy obok materiału.
- **Napisy rozszerzone**: dialogi, kto mówi i ważne dźwięki (1.2.2 A).
- **Audiodeskrypcja** (1.2.5 AA).
- **Transkrypcja** obok lub link do niej.
- PJM to dobra praktyka, nie wymóg ustawy.
- Te formy się nie zastępują, bo każda służy innej grupie.
- Odtwarzacz obsługiwany klawiaturą, z opisanymi przyciskami i obsługą napisów i audiodeskrypcji.

**Błyskające elementy** (`blyskajace-elementy`) · 2.3.1 A
- Nic nie miga częściej niż **3 razy na sekundę**. Najlepiej nie stosować migania wcale. Jeśli się nie da, ostrzeż w tytule lub opisie przed uruchomieniem.

### C. Obsługa i nawigacja

**Obsługa klawiaturą** (`obsluga-klawiatura`) · 2.1.1 A, 2.1.2 A, 2.1.4 A, 2.4.7 AA
- Wszystko działa z klawiatury: Tab, Shift+Tab, Enter, spacja, strzałki.
- **Widoczny fokus.** Wzmocnij domyślny, jeśli ginie na tle. Nie usuwaj `outline` bez zamiennika.
- Fokus tylko na elementach aktywnych (bez `tabindex` > 0 i bez fokusu na `div`).
- Bez pułapek klawiaturowych. Uważaj na zewnętrzne moduły: płatności, social media, mapy.
- Skróty jednoklawiszowe można wyłączyć lub zmienić.

**Kolejność nawigacji i odczytu** (`kolejnosc-nawigacji-i-odczytu`) · 1.3.2 A, 2.4.3 A
- Kolejność w kodzie odpowiada kolejności wizualnej (od lewej do prawej, z góry na dół). Uważaj na `order`, `flex-direction: row-reverse` i pozycjonowanie absolutne.

**Nawigacja** (`nawigacja`) · 2.4.1 A, 2.4.5 AA, 3.2.3 AA, 3.2.4 AA
- Menu i wyszukiwarka są w tym samym miejscu na wszystkich ekranach. Elementy o tej samej funkcji wyglądają i działają tak samo.
- Link „Przejdź do treści” (opcjonalnie też do wyszukiwarki i stopki).
- Do treści da się dojść więcej niż jedną drogą (menu, wyszukiwarka, mapa strony).

**Linki i przyciski** (`linki-i-przyciski`) · 2.4.4 A, 1.4.1 A, 4.1.2 A
- **Link** (`<a href>`) prowadzi w inne miejsce, a **przycisk** (`<button>`) wykonuje akcję. Nie mieszaj ich.
- Treść linku mówi, dokąd prowadzi. Unikaj „czytaj więcej”, „kliknij tutaj” i „pobierz”. Etykieta przycisku mówi, co się stanie.
- Linki o tej samej treści prowadzą w to samo miejsce, bo inaczej nawigacja głosowa się gubi.
- Link do pliku podaje format i rozmiar: „Wzór wniosku (DOCX; 122 kB)”.
- Goły URL jako link tylko w dokumentach do druku.
- Przyciski są wyraźnie odróżnialne od linków i nie za małe (rekomendacja WCAG 2.2: 2.5.8, min. 24×24 px).

**Obsługa ekranem dotykowym** (`obsluga-ekranem-dotykowym`) · 2.5.1 A, 1.3.4 AA
- Każdy gest (szczypanie, przesunięcie) ma odpowiednik w postaci przycisku.
- Nie wymuszaj orientacji pionowej ani poziomej.

**Przewidywalne zachowanie** (`przewidywalne-zachowanie`) · 3.2.1 A, 3.2.2 A
- Fokus ani najechanie kursorem nie zmieniają kontekstu (bez nowego okna, przeniesienia fokusu czy zmiany strony).
- Zmiana wartości pola nie wysyła formularza i nie przenosi użytkownika.

### D. Czas, ruch, komunikaty

**Limity czasowe** (`limity-czasowe`) · 2.2.1 A
- Poinformuj o limicie z wyprzedzeniem i pozwól go wydłużyć. W urzędach to zwykle wygasanie sesji przy długim wniosku.

**Automatycznie zmieniające się treści** (`automatycznie-zmieniajace-sie-tresci`) · 2.2.2 A, 2.3.3 AAA (rekomendacja)
- Preferuj treści przełączane przez użytkownika. Każda ruchoma treść ma widoczny przycisk pauzy obsługiwany klawiaturą i czytnikiem.
- Respektuj `prefers-reduced-motion` (iOS: `isReduceMotionEnabled`, Android: `ANIMATION_DURATION_SCALE = 0`).
- Karuzele według wzorca WAI Carousels.

**Wyskakujące okienka powiadomień** (`wyskakujace-okienka-powiadomien`) · 4.1.2 A, 4.1.3 AA, 2.4.3 A, 1.4.13 AA
- Okno modalne: `<dialog>` lub `role="dialog"` z `aria-modal` i nazwą. Fokus wchodzi do okna, nie ucieka z niego, a po zamknięciu wraca na element wywołujący.
- Zawsze da się je zamknąć (przycisk i Esc). Jeśli działanie w oknie jest konieczne, powiedz to wprost.
- Komunikaty bez okna (zapisano, sesja wygasa za 2 min, wysłano) ogłaszaj przez `role="status"` lub `aria-live`.

**Elementy zewnętrzne** (`elementy-zewnetrzne`) · 2.1.1 A, 2.1.2 A, 4.1.2 A
- Widżety, mapy, `iframe` i moduły płatności mają `title` lub `alt`, działają z klawiatury i nie tworzą pułapek.
- Po dynamicznej zmianie treści alt, role i ARIA nadal są poprawne.

### E. Formularze (`formularze`) · 1.3.1 A, 1.3.5 AA, 3.3.1 A, 3.3.2 A, 3.3.3 AA, 3.3.4 AA

- Każde pole ma widoczną `<label for>`. Placeholder nie zastępuje etykiety.
- Wymagany format jest w etykiecie lub w podpowiedzi przed polem, np. „Data urodzenia (dzień, miesiąc, rok)”.
- **Pole obowiązkowe: sama gwiazdka nie wystarczy.** Napisz to w etykiecie („(wymagane)”) albo podaj zasadę na górze formularza.
- **Grupowanie w kodzie**, nie tylko wizualnie: `<fieldset>` + `<legend>` dla danych adresowych i grup radio i checkbox.
- **Błędy:** samo „Błąd” nie wystarczy. Komunikat mówi, co jest źle i jak to poprawić („Wpisz PESEL: 11 cyfr”).
  - komunikat przy polu, połączony przez `aria-describedby`, a na polu `aria-invalid="true"`,
  - podsumowanie błędów na górze z linkami do pól, a po wysłaniu fokus przechodzi na to podsumowanie,
  - błąd to nie tylko czerwona ramka, ale też tekst i ikona.
- **Potwierdzenie przed wysłaniem** w sprawach prawnych i finansowych: ekran podsumowania z możliwością sprawdzenia, poprawy i rezygnacji. W serwisie urzędowym to obowiązkowy krok.
- `autocomplete` dla danych osobowych (`given-name`, `family-name`, `email`, `tel`, `postal-code`, `street-address`, `bday`).
- Formularz działa z klawiatury, czytnikiem, głosem, na telefonie i z własnymi ustawieniami czcionek i kolorów.
- Dobre praktyki: waliduj po opuszczeniu pola lub przy wysłaniu, a nie w trakcie pisania. Nie blokuj wklejania. Daty w trzech polach. Jedna sprawa na krok, a postęp jako tekst („Krok 2 z 5”).

### F. Prosty język (`prosty-jezyk`) · rekomendacja gov.pl (WCAG 3.1.5 AAA)

Dotyczy etykiet, podpowiedzi, komunikatów błędów i treści.
- Najważniejsza informacja na początku, a dalej treść w logicznej kolejności.
- Zwracaj się bezpośrednio („Wpisz…”, „Masz 14 dni…”). Unikaj strony biernej („dokonano zmiany”) i rzeczowników odczasownikowych („dokonanie zmiany”).
- Zdania **do 15–20 wyrazów**, bez wtrąceń i imiesłowów, w naturalnym szyku.
- Skróty rozwijaj przy pierwszym użyciu, np. „Centralny Ośrodek Informatyki (COI)”. Trudne terminy wyjaśniaj.
- Struktura tekstu: nagłówki, krótkie akapity, listy, tabele.
- Zamiany:

| Zamiast | Pisz |
| --- | --- |
| dokonywać zakupów | kupuj |
| w dniu dzisiejszym | dzisiaj |
| w miesiącu maju | w maju |
| dedykowany | przeznaczony |
| celem | aby |
| komponent | składnik |
| niniejszy | ten |

- Narzędzia: Logios, Jasnopis. **Przed wklejeniem tekstu usuń dane osobowe.**

## Testy

### Automatyczne (wykrywają tylko część problemów)
- `npx @axe-core/cli <url>`, `npx pa11y <url>`, Lighthouse (Accessibility)
- Walidator W3C: https://validator.w3.org
- Kontrast z dwóch kolorów hex:
  ```python
  def L(h):
      c=[int(h[i:i+2],16)/255 for i in (1,3,5)]
      c=[x/12.92 if x<=0.03928 else ((x+0.055)/1.055)**2.4 for x in c]
      return 0.2126*c[0]+0.7152*c[1]+0.0722*c[2]
  def ratio(a,b):
      x,y=sorted([L(a),L(b)],reverse=True); return (x+0.05)/(y+0.05)
  ```

### Ręczne (jeśli nie możesz wykonać, podaj kroki)
- Cała ścieżka tylko klawiaturą, z obserwacją widoczności i kolejności fokusu.
- Czytnik ekranu: VoiceOver (macOS: Cmd+F5, iOS), NVDA (Windows), TalkBack (Android). Sprawdź nagłówki, obszary, etykiety pól, błędy i okna.
- Powiększenie 200% i 400%, szerokość 320 px, zwiększone odstępy w tekście.
- `prefers-reduced-motion` włączone: czy animacje się wyłączają?
- Telefon: orientacja pozioma, gesty mają przyciski.
- Czy na każdej podstronie jest link do deklaracji dostępności?

## Wynik

Zapisz `dostepnosc/<nazwa>-<RRRR-MM-DD>.md` (nie nadpisuj, w razie potrzeby dodaj sufiks):

1. **Podsumowanie:** 3–5 punktów, liczba problemów 🔴/🟠/🟢.
2. **Problemy:** tabela `Priorytet | Element gov.pl | Kryterium WCAG | Miejsce | Problem | Kogo dotyczy | Poprawka`.
3. **Poprawki w kodzie:** gotowe fragmenty przed i po.
4. **Prosty język:** tabela „było → jest” dla etykiet i komunikatów (jeśli dotyczy).
5. **Nie sprawdzono:** co wymaga testu ręcznego lub z czytnikiem.
6. **Do deklaracji dostępności:** lista znanych niezgodności, jeśli nie zostaną naprawione.

W odpowiedzi podaj tylko podsumowanie, 3 najważniejsze poprawki i ścieżkę do raportu.
