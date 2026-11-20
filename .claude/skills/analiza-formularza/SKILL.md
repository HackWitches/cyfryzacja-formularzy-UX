---
name: analiza-formularza
description: Analiza formularza urzędowego (PDF, skan, DOCX, XSD/XML z ePUAP, HTML) lub zbioru formularzy/danych (CSV, JSON, folder plików). Wyciąga listę pól do JSON, wskazuje problemy (prosty język, once-only, walidacja, WCAG, RODO), proponuje uproszczoną wersję i liczy KPI. Używaj, gdy ktoś prosi o analizę, rozbiór, audyt, uproszczenie lub porównanie formularzy albo o analizę danych z formularzy.
argument-hint: <ścieżka do pliku lub folderu>
---

# Analiza formularza / danych

Piszesz po polsku, krótko, listami i tabelami. Analizujesz plik lub folder z `$ARGUMENTS`. Jeśli argument jest pusty, zapytaj jednym zdaniem o plik.

## Zasady

- **Nie zgaduj.** Każde pole ma `pewnosc` (`wysoka` / `srednia` / `niska`). Niepewne rzeczy oznacz i wypisz w sekcji „Do weryfikacji”.
- **Nie wymyślaj podstaw prawnych.** Podaj przepis tylko wtedy, gdy jest w dokumencie. W pozostałych przypadkach wpisz `null`.
- Prawdziwe dane osobowe w plikach (PESEL, adresy) zastąp w raporcie znacznikiem `[dane osobowe]`.
- Zapisuj wyniki w `analizy/` (utwórz folder, jeśli go nie ma). Nie nadpisuj istniejących plików, tylko dodaj sufiks `-2`, `-3` itd.

## Tryb A: jeden formularz

### 1. Wczytanie
- PDF: `pdftotext -layout` (jeśli jest), w przeciwnym razie odczytaj plik bezpośrednio. Sprawdź, czy PDF ma pola AcroForm.
- Skan lub obraz: odczytaj wizualnie i oznacz pewność niżej.
- XSD/XML: elementy, typy, `minOccurs` (wymagalność), `enumeration`, wzorce `pattern`.
- DOCX/HTML: tekst, tabele i pola.

### 2. Inwentaryzacja pól
Dla każdego pola ustal:

| Klucz | Opis |
| --- | --- |
| `id` | snake_case, bez polskich znaków |
| `etykieta` | oryginalna etykieta z formularza |
| `sekcja` | nazwa sekcji lub kroku |
| `typ` | `text`, `number`, `date`, `select`, `radio`, `checkbox`, `pesel`, `nip`, `regon`, `adres`, `email`, `telefon`, `plik`, `podpis` |
| `wymagane` | `true` / `false` / `null` |
| `opcje` | dla `select`, `radio` i `checkbox` |
| `walidacja` | format, długość, suma kontrolna (PESEL, NIP, REGON) |
| `warunek` | kiedy pole się pojawia, np. „gdy pole X = tak” |
| `zrodlo_danych` | czy państwo już zna tę wartość: `PESEL`, `CEIDG`, `KRS`, `REGON`, `TERYT`, `CEPiK`, `profil zaufany`, `null` |
| `podstawa_prawna` | tylko jeśli jest w dokumencie |
| `pewnosc` | `wysoka` / `srednia` / `niska` |

### 3. Problemy
Sprawdź i wypisz z priorytetem (🔴 blokujące, 🟠 ważne, 🟢 drobne):
- **Prosty język:** urzędowe sformułowania, skróty, zdania dłuższe niż ok. 20 słów.
- **Once-only:** pola, które da się pobrać z rejestrów.
- **Zbędne pola:** dane niepotrzebne do celu usługi (RODO, minimalizacja).
- **Walidacja:** brak formatu, brak podpowiedzi, ogólny komunikat „Błąd”.
- **Dostępność (WCAG 2.1 AA):** etykiety, kolejność, instrukcje oparte tylko na kolorze lub układzie.
- **Logika:** sprzeczne lub zależne pola bez warunków, duplikaty.
- **Struktura:** zbyt dużo pól na jednym ekranie (zalecane: jedna sprawa na krok).

### 4. Propozycja uproszczenia
- Podział na kroki, jedna sprawa na krok, w stylu mObywatela.
- Nowe etykiety prostym językiem (tabela: stara etykieta → nowa).
- Lista pól do autouzupełnienia i do usunięcia.

### 5. Wynik
- `analizy/<nazwa>.json`: `{ "formularz": {...meta}, "kroki": [ { "tytul", "pola": [...] } ], "problemy": [...] }`
- `analizy/<nazwa>.md` z sekcjami: Podsumowanie (3–5 punktów), KPI, Pola (tabela), Problemy, Propozycja, Do weryfikacji.

**KPI:** liczba pól, % pól możliwych do autouzupełnienia, % pól do usunięcia, liczba problemów 🔴/🟠/🟢, % pól z pewnością `niska`.

## Tryb B: wiele formularzy lub dane (folder, CSV, JSON)

1. Uruchom tryb A dla każdego formularza. Przy dużych zbiorach rób najpierw tylko inwentaryzację pól.
2. **Porównanie:** wspólne pola (po `typ` i znaczeniu etykiety, nie po dosłownym brzmieniu) i formularze prawie identyczne między urzędami, np. ta sama usługa w różnych gminach. Na tej podstawie zaproponuj **wspólne wzory**.
3. **Dane tabelaryczne (CSV/JSON):** liczba rekordów, kolumny i typy, braki danych, duplikaty, rozkłady najważniejszych pól, anomalie. Do obliczeń użyj Pythona (`python3 -I`) i podaj kod, który to policzył.
4. **Wynik:** `analizy/zbior-<nazwa>.md` z tabelą zbiorczą (formularz × KPI), listą wspólnych wzorów i top 10 problemów w całym zbiorze, oraz `analizy/zbior-<nazwa>.json`.

## Na koniec
W odpowiedzi podaj 3–5 najważniejszych wniosków i ścieżki do plików. Nie wklejaj całego raportu.
