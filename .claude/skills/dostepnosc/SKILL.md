---
name: dostepnosc
description: Audyt dostępności cyfrowej (WCAG 2.1 AA, ustawa o dostępności cyfrowej podmiotów publicznych) dla stron, formularzy, komponentów i projektów. Działa na kodzie (HTML, React, CSS), adresie URL, zrzucie ekranu lub pliku Figma. Zwraca priorytetową listę problemów z numerem kryterium WCAG i konkretną poprawką, a na prośbę wprowadza bezpieczne poprawki w kodzie. Używaj przy pytaniach o dostępność, accessibility, a11y, WCAG, kontrast, czytniki ekranu, obsługę klawiaturą lub przed oddaniem ekranu czy demo.
argument-hint: <plik | folder | URL | link do Figmy> [--fix]
---

# Audyt dostępności (serwis publiczny)

Piszesz po polsku, krótko, listami i tabelami. Audytujesz cel z `$ARGUMENTS`. Jeśli argument jest pusty, zapytaj jednym zdaniem, co sprawdzić.

## Kontekst prawny (dlaczego to obowiązek)

- Podmioty publiczne podlegają **ustawie z 4 kwietnia 2019 r. o dostępności cyfrowej stron internetowych i aplikacji mobilnych podmiotów publicznych**. Wymagany poziom: **WCAG 2.1 AA** (zgodnie z normą EN 301 549).
- Każdy serwis publiczny musi mieć **deklarację dostępności**.
- WCAG 2.2 nie jest jeszcze wymogiem prawnym, ale jej kryteria (np. 2.4.11, 2.5.8, 3.3.7, 3.3.8) traktuj jako **rekomendację** i oznaczaj osobno.
- Wytyczne gov.pl dla formularzy: https://www.gov.pl/web/dostepnosc-cyfrowa/formularze

## Zasady

- **Nigdy nie pisz „zgodne z WCAG”** na podstawie samych testów automatycznych. Automaty wykrywają tylko część problemów. Zawsze wypisz, czego nie sprawdzono.
- Każdy problem ma: numer i nazwę kryterium WCAG, poziom (A/AA), miejsce (plik:linia, selektor lub nazwa warstwy), **kogo dotyczy** (np. osoby niewidome, słabowidzące, poruszające się tylko klawiaturą, z niepełnosprawnością poznawczą) oraz konkretną poprawkę.
- Priorytety: 🔴 blokuje wykonanie zadania · 🟠 utrudnia · 🟢 drobne lub rekomendacja WCAG 2.2.
- Z `--fix`: wprowadzaj tylko bezpieczne poprawki (atrybuty, etykiety, semantyka, kontrast w tokenach). Przed zmianą układu lub logiki zapytaj.

## Tryby

### Kod (HTML, React, CSS, design system)
1. Przejrzyj komponenty i style. Najpierw tokeny kolorów, bo kontrast w tokenach naprawia wszystko naraz.
2. Jeśli aplikacja daje się uruchomić, odpal testy automatyczne (sekcja niżej).
3. Przejdź checklistę.

### URL
1. `npx @axe-core/cli <url>` oraz `npx pa11y <url>` (jeśli dostępne).
2. Odczytaj stronę i przejdź checklistę dla tego, czego nie da się sprawdzić automatem.

### Projekt (zrzut ekranu lub Figma)
- Sprawdź kontrast (próbkuj kolory), rozmiary celów dotyku, etykiety, stany (focus, błąd, disabled), komunikaty błędów, kolejność czytania i to, czy informacja nie jest przekazywana wyłącznie kolorem.
- Wypisz, co trzeba zapisać w specyfikacji dla developerów: kolejność fokusu, teksty alternatywne, role i nazwy dostępne.

## Checklista (WCAG 2.1 AA)

### Postrzegalność
- 1.1.1 Tekst alternatywny dla obrazów i ikon (ikony dekoracyjne: `alt=""` lub `aria-hidden="true"`).
- 1.3.1 Semantyka: nagłówki po kolei, listy, tabele z `<th>`, pola powiązane z etykietą.
- 1.3.5 `autocomplete` dla danych osobowych (`given-name`, `family-name`, `email`, `tel`, `postal-code`, `street-address`, `bday`).
- 1.4.1 Informacja nie jest przekazywana wyłącznie kolorem (np. błąd = kolor + ikona + tekst).
- 1.4.3 Kontrast tekstu: **4,5:1**, a dla dużego tekstu (od 24 px lub od 18,66 px pogrubione) **3:1**.
- 1.4.11 Kontrast elementów UI i fokusu: **3:1** (ramki pól, ikony, stany).
- 1.4.10 Reflow: działa przy szerokości 320 px bez przewijania w poziomie.
- 1.4.4 i 1.4.12 Powiększenie 200% i zwiększone odstępy w tekście nie ucinają treści.

### Funkcjonalność
- 2.1.1 i 2.1.2 Wszystko działa z klawiatury, bez pułapek.
- 2.4.7 Widoczny fokus (nie usuwaj `outline` bez zamiennika).
- 2.4.3 Logiczna kolejność fokusu.
- 2.4.1 Link „Przejdź do treści”.
- 2.4.2 i 2.4.6 Unikalny `<title>` każdego kroku i opisowe nagłówki („Krok 2 z 5: Adres”).
- 2.2.1 **Limity czasu sesji:** ostrzeżenie przed wygaśnięciem i możliwość przedłużenia. W urzędach to częsty problem.
- 2.5.3 Nazwa dostępna zawiera widoczną etykietę.

### Zrozumiałość
- 3.1.1 `lang="pl"`, a fragmenty w innym języku mają własny `lang`.
- 3.2.2 Zmiana wartości pola nie przenosi użytkownika ani nie wysyła formularza.
- 3.3.1 Błędy opisane tekstem przy polu.
- 3.3.2 Etykiety i instrukcje: format podany przed polem, a nie dopiero w błędzie.
- 3.3.3 Komunikat błędu mówi, **jak poprawić** („Wpisz PESEL, 11 cyfr”), a nie tylko „Błąd”.
- 3.3.4 **Sprawy prawne i finansowe:** podsumowanie przed wysłaniem z możliwością poprawy. W serwisie urzędowym to obowiązkowy krok.

### Solidność
- 4.1.2 Nazwa, rola i wartość: natywne elementy zamiast `div` z `onClick`. ARIA tylko tam, gdzie HTML nie wystarcza.
- 4.1.3 Komunikaty statusu (zapisano, błędy, ładowanie) przez `role="status"` lub `aria-live`.

### Formularze: wzorce do sprawdzenia
- Każde pole ma `<label for>`. Placeholder nie zastępuje etykiety.
- Grupy radio i checkbox w `<fieldset>` z `<legend>`.
- Pola wymagane oznaczone tekstem („wymagane”) albo informacja „wszystkie pola są wymagane, chyba że oznaczono inaczej”.
- Błędy: **podsumowanie na górze** z linkami do pól, komunikat przy polu połączony przez `aria-describedby`, na polu `aria-invalid="true"`, a po wysłaniu fokus przechodzi na podsumowanie.
- Nie waliduj agresywnie w trakcie pisania. Waliduj po opuszczeniu pola lub przy wysłaniu.
- Nie blokuj wklejania (PESEL, numer konta).
- Daty: trzy pola (dzień, miesiąc, rok) albo jasno podany format.
- Jedna sprawa na krok. Pasek postępu dostępny jako tekst („Krok 2 z 5”).
- Prosty język w etykietach i podpowiedziach.

## Testy

### Automatyczne (część problemów)
- `npx @axe-core/cli <url>`
- `npx pa11y <url>`
- Lighthouse (kategoria Accessibility)
- Kontrast z dwóch kolorów hex:
  ```python
  def L(h):
      c=[int(h[i:i+2],16)/255 for i in (1,3,5)]
      c=[x/12.92 if x<=0.03928 else ((x+0.055)/1.055)**2.4 for x in c]
      return 0.2126*c[0]+0.7152*c[1]+0.0722*c[2]
  def ratio(a,b):
      x,y=sorted([L(a),L(b)],reverse=True); return (x+0.05)/(y+0.05)
  ```

### Ręczne (podaj kroki, jeśli nie możesz wykonać)
- Przejście całej ścieżki samym klawiszem Tab, Shift+Tab, Enter i spacją.
- Czytnik ekranu: VoiceOver (macOS: Cmd+F5) lub NVDA (Windows).
- Powiększenie 200% i 400% oraz szerokość 320 px.

## Wynik

Zapisz `dostepnosc/<nazwa>-<RRRR-MM-DD>.md` (nie nadpisuj, w razie potrzeby dodaj sufiks):

1. **Podsumowanie:** 3–5 punktów i liczba problemów 🔴/🟠/🟢.
2. **Problemy:** tabela `Priorytet | Kryterium WCAG | Miejsce | Problem | Kogo dotyczy | Poprawka`.
3. **Poprawki w kodzie:** gotowe fragmenty przed i po.
4. **Nie sprawdzono:** co wymaga testu ręcznego lub z czytnikiem.
5. **Do deklaracji dostępności:** lista znanych niezgodności, jeśli nie zostaną naprawione.

W odpowiedzi podaj tylko podsumowanie, 3 najważniejsze poprawki i ścieżkę do raportu.
