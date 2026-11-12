# Ściąga promptów dla PM

[English version](PROMPTS.en.md)

Prompty do skopiowania. W nawiasach `[...]` wstaw swoje dane. Tam, gdzie istnieje skill z tego repo, prompt go uruchamia; resztę model zrobi dobrze bez skilla, jeśli dostanie właściwe ograniczenia.

## Zasady, które robią różnicę

1. **Daj dane, nie opis danych.** Wklej transkrypt, CSV, ticket, maila. „Mamy dużo churnu” daje ogólniki; 200 wierszy eksportu daje wnioski.
2. **Nazwij decyzję.** „Pomóż mi zdecydować, czy X czy Y do piątku” zamiast „przeanalizuj X”.
3. **Każ najpierw pytać.** „Zanim zaczniesz, zadaj mi do 3 pytań o to, czego nie wiesz.” Odcina zgadywanie.
4. **Każ oznaczać założenia.** „To, czego nie wiesz z moich danych, oznacz jako [ZAŁOŻENIE], nie pisz jako fakt.”
5. **Ogranicz długość.** „Maks. 1 strona” / „5 punktów”. Bez limitu dostaniesz raport.
6. **Poproś o krytykę, nie o pochwałę.** „Co jest najsłabsze?” działa lepiej niż „co o tym myślisz?”.

---

## Discovery

**Synteza wielu wywiadów**
```
Poniżej [N] transkryptów wywiadów z [segment]. Wyciągnij:
1. Problemy, które padły u co najmniej 2 osób (z liczbą osób i cytatem dla każdego).
2. Co robią dziś zamiast naszego produktu i ile ich to kosztuje (czas/pieniądze).
3. Rzeczy, które mnie zaskoczą — sprzeczne z założeniem: [twoje założenie].
Nie uogólniaj z jednej wypowiedzi. Jeśli coś powiedziała jedna osoba, napisz to wprost.
[transkrypty]
```

**Scenariusz wywiadu** → skill `interview-script`
```
/pm-skill:interview-script Wywiady z [segment] o tym, jak dziś [czynność]. Decyzja, którą mają wesprzeć: [decyzja]. 30 minut.
```

**Klasteryzacja feedbacku / zgłoszeń**
```
Poniżej [N] zgłoszeń od klientów. Pogrupuj je według PROBLEMU, nie według proponowanego rozwiązania.
Dla każdej grupy: nazwa problemu, liczba zgłoszeń, 2 cytaty, jaki segment zgłasza najczęściej.
Na końcu: które prośby o feature to w rzeczywistości ten sam problem.
[zgłoszenia]
```

**Mapa założeń**
```
Pomysł: [opis]. Wypisz założenia, które muszą być prawdziwe, żeby to zadziałało
(wartość dla klienta, używalność, opłacalność, wykonalność).
Dla każdego: co jeśli jest fałszywe, jak pewni jesteśmy na podstawie [dowody], najtańszy test w tydzień.
Posortuj: najpierw te, które zabijają pomysł i których najmniej jesteśmy pewni.
```

**Ankieta** → skill `survey-design`
```
/pm-skill:survey-design Decyzja: [decyzja]. Do kogo: [grupa]. Kanał: [mail / in-app]. Maks. 5 pytań.
```

---

## Strategia i decyzje

**Notatka decyzyjna (decision memo)**
```
Muszę zdecydować: [decyzja] do [data]. Opcje: [A], [B], [C (nic nie robić)].
Dla każdej opcji: co zyskujemy, co tracimy, co musi być prawdą, żeby była najlepsza, koszt odwrócenia decyzji.
Zarekomenduj jedną i napisz, jaki sygnał w ciągu [okres] powinien nas skłonić do zmiany zdania.
Maks. 1 strona.
```

**Red-team planu** → skill `strategy-red-team`
```
/pm-skill:strategy-red-team [wklej plan / strategię / roadmapę]
```

**Pre-mortem** → skill `pre-mortem`
```
/pm-skill:pre-mortem [wklej PRD lub plan launchu]. Launch: [data].
```

**Steelman przeciwnego stanowiska**
```
Uważam, że [stanowisko]. Napisz najmocniejszy możliwy argument PRZECIW, tak jak przedstawiłby go
kompetentny [CFO / CTO / head of sales]. Potem: które z tych argumentów naprawdę podważają moje stanowisko, a które nie.
```

**Analiza konkurenta (z wyszukiwaniem)**
```
Sprawdź w sieci [konkurent]: cennik, ostatnie 6 miesięcy zmian w produkcie, na co narzekają klienci (G2, Reddit, Capterra).
Każde twierdzenie z linkiem do źródła. Na końcu: 3 rzeczy, w których realnie wygrywają z nami w [nasz segment], i 3, w których my.
```

---

## Specyfikacja i delivery

**PRD** → skill `create-prd`
```
/pm-skill:create-prd [problem, dla kogo, jak zmierzymy sukces, linki/pliki]
```

**Przegląd istniejącego PRD**
```
/pm-skill:create-prd Przejrzyj ten PRD: najpierw luki blokujące decyzję, potem co można wyciąć. [PRD]
```

**Cięcie zakresu do MVP**
```
Oto lista funkcji na pierwszą wersję: [lista]. Cel pierwszej wersji: sprawdzić, czy [hipoteza].
Które funkcje są niezbędne do sprawdzenia tej hipotezy, a które nie? Dla wyciętych napisz, czym je tymczasowo zastąpić
(ręcznie, mock, zewnętrzne narzędzie).
```

**Kryteria akceptacji i edge case'y**
```
User story: [story]. Napisz kryteria akceptacji w formacie Given/When/Then.
Potem osobno: 10 przypadków brzegowych, o których zespół najpewniej zapomni (puste dane, uprawnienia, równoległe akcje,
strefy czasowe, limity, błędy sieci, wycofanie zmiany). Oznacz, które wymagają decyzji produktowej, a które są czysto techniczne.
```

**Rozbicie epiki na stories**
```
Epika: [opis]. Rozbij na stories, z których każda da się wdrożyć i pokazać użytkownikowi osobno (pionowe plastry, nie warstwy techniczne).
Kolejność: najpierw ta, która najszybciej pokaże, czy cała epika ma sens.
```

---

## Dane i metryki

**Definicja metryki**
```
Chcę mierzyć [co]. Zaproponuj dokładną definicję: licznik, mianownik, okno czasowe, kogo wykluczamy.
Wypisz 3 sposoby, w jakie ta metryka może rosnąć, choć produkt się pogarsza, i metrykę-strażnika (guardrail) dla każdego.
```

**Drzewo metryk**
```
Nasza główna metryka: [metryka]. Rozpisz ją na składowe (np. ruch × konwersja × częstotliwość), aż do poziomu,
na który jeden zespół może wpłynąć w kwartał. Przy każdej gałęzi: który zespół ją posiada i skąd bierzemy dane.
```

**SQL ze schematu**
```
Schemat: [CREATE TABLE ... / opis tabel]. Dialekt: [Postgres/BigQuery].
Napisz zapytanie: [pytanie biznesowe]. Wyjaśnij założenia (np. jak liczysz aktywnego użytkownika, strefa czasowa)
i dodaj zapytanie kontrolne, którym sprawdzę, że wynik jest sensowny.
```

**Analiza testu A/B** → skill `ab-test-analysis`
```
/pm-skill:ab-test-analysis Hipoteza: [..]. Metryka główna: [..]. Guardraile: [..]. Dane: [CSV / liczby per wariant].
```

**Dochodzenie: metryka spadła**
```
[Metryka] spadła o [X%] od [data]. Dane: [eksport / wykres / liczby per segment].
Wypisz możliwe przyczyny w kolejności: błąd danych/trackingu → zmiana mixu ruchu → sezonowość → zmiana w produkcie → zmiana u klientów.
Dla każdej: jak ją potwierdzić lub wykluczyć w 30 minut.
```

---

## Komunikacja

**Status dla zarządu**
```
Napisz update dla [odbiorca] w formacie: najważniejsza informacja w pierwszym zdaniu, potem: postęp vs plan,
ryzyka z tym, czego od nich potrzebujemy, następne kroki. Maks. 150 słów. Bez przymiotników typu „świetny”, „znaczący”.
Fakty: [notatki]
```

**Odmowa interesariuszowi**
```
[Osoba/dział] prosi o [prośba]. Nie zrobimy tego teraz, bo [powód]. Napisz odpowiedź, która:
mówi „nie” w pierwszym zdaniu, wyjaśnia powód w kategoriach ich celu, proponuje alternatywę lub termin powrotu do tematu.
Ton: [bezpośredni / ciepły]. Maks. 100 słów.
```

**Release notes**
```
Oto lista zmian z ticketów: [lista]. Przepisz na notatkę dla użytkowników: korzyść zamiast zmiany technicznej,
bez numerów ticketów i nazw wewnętrznych. Osobno: zmiany, które wymagają działania od użytkownika.
```

**Notatka ze spotkania**
```
Transkrypt: [..]. Wyciągnij tylko: decyzje (kto zdecydował), zadania (kto, co, do kiedy), otwarte pytania.
Jeśli zadanie nie ma właściciela lub terminu, oznacz [BRAK WŁAŚCICIELA] / [BRAK TERMINU]. Nie streszczaj dyskusji.
```

---

## Krytyka własnej pracy

**Sceptyczny recenzent**
```
Jesteś sceptycznym [CTO / CFO / head of sales], który ma 5 minut na ten dokument. Jakie 3 pytania zadasz na spotkaniu,
na które dokument nie odpowiada? Co uznasz za niewiarygodne?
[dokument]
```

**Spójność dokumentu**
```
Sprawdź, czy ten dokument jest wewnętrznie spójny: czy cele zgadzają się z metrykami, rozwiązanie z problemem,
zakres z harmonogramem. Wypisz tylko niespójności z cytatami obu miejsc.
[dokument]
```

**Skracanie**
```
Skróć ten tekst o połowę bez utraty żadnej decyzji, liczby ani zadania. Pokaż, co usunąłeś.
[tekst]
```
