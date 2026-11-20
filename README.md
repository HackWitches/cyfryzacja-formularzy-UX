# Cyfryzacja formularzy

HackNation 2026 · zadanie partnerskie **Centralnego Ośrodka Informatyki (COI)** · nagroda główna 25 000 zł

> Stan na 20.11.2026. Pełna treść zadania, kryteria oceny i wagi zostaną opublikowane **21.11 o 11:00**. Wtedy aktualizujemy ten plik.

## Zadanie

> „Jak sztuczna inteligencja może wspierać rozwój nowoczesnych usług publicznych?”

- Cel: usprawnić proces cyfryzacji, żeby **„droga od potrzeby do wygodnej usługi online”** była prostsza i szybsza.
- Źródło: https://hacknation.pl/zadania

## Nasz kierunek (roboczo)

Asystent AI, który przenosi formularze urzędów (PDF, skan, DOCX, XSD z ePUAP) do Generatora Formularzy e-Usług COI:

1. **Wejście:** PDF, skan, DOCX lub XSD
2. **Ekstrakcja:** pola, typy, wymagalność, sekcje, warunki → JSON
3. **Wzbogacenie:** autouzupełnianie z rejestrów, usunięcie pól, które państwo już zna, podstawa prawna (ELI)
4. **Kontrola:** Style Guide gov.pl, WCAG 2.1 AA, art. 63 KPA, spójność logiki
5. **Podgląd i eksport:** formularz w stylu mObywatela + JSON do Generatora
6. **Wsad i duplikaty:** wiele formularzy z wielu gmin → wspólne wzory

Człowiek zatwierdza każdy wynik.

## Harmonogram

| Kiedy | Co |
| --- | --- |
| sob 21.11, 11:00 | Publikacja zadań i kryteriów, start zgłoszeń w HackTribe |
| sob 21.11, 20:00 | **DRAFT**: nazwa projektu i zespołu, repo, nr stolika, opis. Potem nie da się zmienić zadania |
| ndz 22.11, 11:00 | **Finalne zgłoszenie**: prezentacja i materiały do sprawdzenia |
| ndz 22.11, 14:45 | Finaliści (top 5 z zadania) |
| ndz 22.11, 15:15 | Prezentacje finalistów przed jury |
| ndz 22.11, 17:15 | Ogłoszenie zwycięzców |

Miejsce: BCTW, ul. Gdańska 187, Bydgoszcz (tylko stacjonarnie).

## Zasady, o których pamiętamy

- Demo zalecane **do 2 minut**. Jurorzy mają kilka minut na ocenę wstępną.
- Wynik oceny wstępnej nie przechodzi do finału.
- Wcześniej przygotowany kod i projekty UI są dozwolone, ale trzeba je zacytować w prezentacji i w kodzie.
- Autorskie prawa majątkowe do zwycięskiego projektu mogą przejść na COI (możliwy wyjątek: open source).

## Kontekst

- Wzory dokumentów elektronicznych (XML/XSD) trafiają do CRWDE w ePUAP (art. 19b ustawy o informatyzacji).
- Narzędzia COI: Generator Formularzy mObywatel, nFORMS, formularze ePUAP.
- Mapa Życia (MC + COI, 05.2026): 179 z 344 usług dla obywateli zdigitalizowanych.
- Wymogi: WCAG 2.1 AA (ustawa o dostępności cyfrowej), RODO (minimalizacja danych), KRI.
- Benchmark: [form-extractor-prototype](https://github.com/timpaul/form-extractor-prototype) (GDS). LLM zamienia PDF na formularz GOV.UK Forms.

## Otwarte pytania do mentorów COI

- Kto jest docelowym użytkownikiem: urzędnik, weryfikator w COI czy obywatel?
- Jaki jest format importu Generatora? Czy jest przykładowy eksport?
- Jakie formularze wejściowe udostępnią?
- Jakie modele są dozwolone: tylko PLLuM/Bielik czy też zewnętrzne API?
- Jakie są kryteria oceny, wagi i skład jury?

## Struktura repo

- `design-system/`: komponenty i style (styl mObywatela)
- `.claude/skills/analiza-formularza/`: skill Claude Code do analizy formularzy i danych (`/analiza-formularza <plik>`)
