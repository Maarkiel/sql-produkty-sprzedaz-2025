SQL Produkty i Sprzedaż 2025

Opis projektu

Projekt przedstawia przykładową bazę danych SQLite służącą do nauki języka SQL oraz podstaw analizy danych sprzedażowych. Baza obejmuje katalog produktów, dane klientów, dostawców, magazynów, zamówień i płatności.

Dane sprzedażowe dotyczą roku 2025. Produkty posiadają między innymi numery EAN, kody SKU, opisy, marki, kategorie, ceny zakupu i sprzedaży oraz dodatkowe parametry, takie jak kolor, rozmiar, materiał i waga.

Projekt został przygotowany jako praktyczne ćwiczenie obejmujące filtrowanie danych, sortowanie, agregacje, łączenie tabel za pomocą JOIN, tworzenie raportów oraz analizę sprzedaży według kategorii, produktów, miesięcy i kanałów dystrybucji.

Technologie

•
SQLite

•
SQL

•
PowerShell

•
Visual Studio Code

•
Git i GitHub

Zawartość bazy

Baza zawiera:

•
60 produktów,

•
400 klientów,

•
3 000 zamówień z 2025 roku,

•
7 522 pozycji zamówień,

•
6 kategorii produktów,

•
6 dostawców,

•
stany magazynowe w dwóch magazynach,

•
płatności powiązane z zamówieniami,

•
widok v_sprzedaz_szczegoly do wygodnej analizy sprzedaży.

Struktura projektu

Plain Text


.
├── produkty_sprzedaz_2025.sqlite
├── produkty_sprzedaz_2025.sql
├── README.md
└── docs/
    └── screenshots/
        ├── 01-uruchomienie-bazy.png
        ├── 02-lista-tabel.png
        ├── 03-schema-produktow.png
        ├── 04-produkty-z-ean.png
        ├── 05-produkty-z-cenami.png
        ├── 06-join-kategorie.png
        ├── 07-join-dostawcy.png
        ├── 08-sprzedaz-kategorie.png
        ├── 09-zysk-kategorie.png
        ├── 10-top-5-produktow.png
        ├── 11-wszystkie-produkty.png
        ├── 12-kanaly-dystrybucji.png
        ├── 13-udzial-kanalow.png
        ├── 14-sprzedaz-miesieczna.png
        ├── 15-najlepszy-miesiac.png
        ├── 16-stany-magazynowe.png
        ├── 17-klienci-b2c-b2b.png
        └── 18-podsumowanie-projektu.png



Najważniejsze tabele

Tabela
Opis
produkty
Katalog produktów z EAN, SKU, cenami i parametrami
kategorie
Kategorie produktowe i stawki VAT
dostawcy
Dostawcy produktów, kraje i oceny
klienci
Klienci indywidualni B2C oraz firmy B2B
zamowienia
Nagłówki zamówień, daty, kanały i statusy
pozycje_zamowien
Produkty oraz ilości w poszczególnych zamówieniach
platnosci
Informacje o płatnościach
stany_magazynowe
Dostępne ilości produktów w magazynach
v_sprzedaz_szczegoly
Widok łączący sprzedaż, produkty, klientów i kategorie




Uruchomienie bazy w Windows

Przejdź w PowerShellu do katalogu projektu:

Plain Text


cd "C:\Users\matty\Downloads\sql-produkty-sprzedaz-2025"



Uruchom plik bazy SQLite:

Plain Text


C:\sqlite\sqlite3.exe ".\produkty_sprzedaz_2025.sqlite"



Po pojawieniu się promptu sqlite> włącz nagłówki i tryb kolumnowy:

SQL


.headers on
.mode column



Sprawdź dostępne tabele:

SQL


.tables



Wyświetl pięć przykładowych produktów:

SQL


SELECT produkt_id,
       ean,
       sku,
       nazwa,
       marka,
       cena_sprzedazy_netto
FROM produkty
LIMIT 5;



Przykładowe zapytania

Sprzedaż i zysk według kategorii

SQL


SELECT
    kategoria,
    ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto,
    ROUND(SUM(cena_zakupu_netto * ilosc), 2) AS koszt_zakupu,
    ROUND(SUM(wartosc_netto - cena_zakupu_netto * ilosc), 2) AS zysk_netto,
    SUM(ilosc) AS sprzedane_sztuki
FROM v_sprzedaz_szczegoly
WHERE status = 'Zrealizowane'
  AND data_zamowienia >= '2025-01-01'
  AND data_zamowienia < '2026-01-01'
GROUP BY kategoria
ORDER BY zysk_netto DESC;



Pięć najlepiej sprzedających się produktów

SQL


SELECT
    produkt,
    ean,
    marka,
    kategoria,
    SUM(ilosc) AS sprzedane_sztuki,
    ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto
FROM v_sprzedaz_szczegoly
WHERE status = 'Zrealizowane'
GROUP BY produkt_id, produkt, ean, marka, kategoria
ORDER BY sprzedane_sztuki DESC,
         sprzedaz_netto DESC
LIMIT 5;



Sprzedaż według kanałów dystrybucji

SQL


SELECT
    kanal,
    COUNT(DISTINCT zamowienie_id) AS liczba_zamowien,
    SUM(ilosc) AS sprzedane_sztuki,
    ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto,
    ROUND(SUM(wartosc_brutto), 2) AS sprzedaz_brutto
FROM v_sprzedaz_szczegoly
WHERE status = 'Zrealizowane'
GROUP BY kanal
ORDER BY sprzedaz_netto DESC;



Jak interpretować zysk

W projekcie zysk netto liczony jest według wzoru:

Plain Text


zysk netto = sprzedaż netto − koszt zakupu



Koszt zakupu jest liczony jako:

Plain Text


cena zakupu netto × liczba sprzedanych sztuk



W raportach uwzględniane są zamówienia ze statusem Zrealizowane. Zamówienia anulowane i zwrócone nie są traktowane jako faktyczna sprzedaż.

Galeria wyników

## Dokumentacja wizualna

### 1. Uruchomienie bazy

Screen pokazuje uruchomienie bazy SQLite z poziomu terminala PowerShell.

![Uruchomienie bazy](docs/screenshots/1.png)

### 2. Konfiguracja SQLite

Screen pokazuje ustawienie nagłówków i trybu kolumnowego.

![Konfiguracja SQLite](docs/screenshots/2.png)

### 3. Lista tabel

Screen przedstawia tabele oraz widok dostępne w bazie danych.

![Lista tabel](docs/screenshots/3.png)

### 4. Schemat tabeli produktów

Screen pokazuje strukturę tabeli `produkty`, w tym EAN, SKU, ceny i parametry produktów.

![Schemat produktów](docs/screenshots/4.png)

### 5. Produkty i identyfikatory

Screen przedstawia przykładowe rekordy produktów wraz z identyfikatorami EAN i SKU.

![Produkty i identyfikatory](docs/screenshots/5.png)

### 6. Produkty i ceny

Screen pokazuje zapytanie wybierające produkty oraz ich ceny sprzedaży netto.

![Produkty i ceny](docs/screenshots/6.png)

### 7. Połączenie z kategoriami

Screen prezentuje użycie `JOIN` do połączenia produktów z kategoriami.

![Połączenie z kategoriami](docs/screenshots/7.png)

### 8. Połączenie z dostawcami

Screen pokazuje dane produktów połączone z informacjami o dostawcach.

![Połączenie z dostawcami](docs/screenshots/8.png)

### 9. Sprzedaż według kategorii

Screen przedstawia sprzedaż netto oraz liczbę sprzedanych sztuk w kategoriach.

![Sprzedaż według kategorii](docs/screenshots/9.png)

### 10. Zysk według kategorii

Screen pokazuje koszt zakupu i zysk netto dla poszczególnych kategorii.

![Zysk według kategorii](docs/screenshots/10.png)

### 11. Top 5 produktów

Screen prezentuje pięć produktów o największej liczbie sprzedanych sztuk.

![Top 5 produktów](docs/screenshots/11.png)

### 12. Wszystkie produkty według sprzedaży

Screen pokazuje pełny ranking produktów uporządkowany według liczby sprzedanych sztuk.

![Ranking produktów](docs/screenshots/12.png)

### 13. Sprzedaż według kanałów

Screen przedstawia liczbę zamówień, sprzedane sztuki i sprzedaż według kanałów dystrybucji.

![Kanały dystrybucji](docs/screenshots/13.png)

### 14. Udział kanałów w sprzedaży

Screen pokazuje udział procentowy kanałów dystrybucji w sprzedaży netto.

![Udział kanałów](docs/screenshots/14.png)

### 15. Sprzedaż miesięczna

Screen przedstawia sprzedaż w kolejnych miesiącach 2025 roku.

![Sprzedaż miesięczna](docs/screenshots/15.png)

### 16. Najlepszy miesiąc

Screen pokazuje miesiąc z najwyższą wartością sprzedaży netto.

![Najlepszy miesiąc](docs/screenshots/16.png)

### 17. Stany magazynowe

Screen prezentuje produkty, których stan magazynowy spadł poniżej ustalonego progu.

![Stany magazynowe](docs/screenshots/17.png)

### 18. Podsumowanie projektu

Screen przedstawia końcowe podsumowanie pracy z bazą danych i wykonanych analiz.

![Podsumowanie projektu](docs/screenshots/18.png)



Ćwiczenia SQL

1.
Wyświetl aktywne produkty marki Nova.

2.
Znajdź produkty w cenie netto od 50 do 150 zł.

3.
Policz produkty w każdej kategorii.

4.
Oblicz średnią cenę produktu według kategorii.

5.
Oblicz sprzedaż netto według miesięcy 2025 roku.

6.
Znajdź pięć produktów z największą liczbą sprzedanych sztuk.

7.
Oblicz zysk według kategorii.

8.
Porównaj sprzedaż klientów B2C i B2B.

9.
Znajdź produkty poniżej progu zamówienia w magazynie.

10.
Oblicz udział kanałów dystrybucji w sprzedaży netto.

Odtworzenie bazy z pliku SQL

Jeżeli chcesz utworzyć nową kopię bazy z tekstowego dumpa SQL, wykonaj w PowerShellu:

Plain Text


C:\sqlite\sqlite3.exe ".\kopia_bazy.sqlite" < ".\produkty_sprzedaz_2025.sql"



Plik .sqlite jest gotową bazą do pracy, natomiast plik .sql zawiera instrukcje tworzące tabele, indeksy, widok i dane.

Cel edukacyjny

Projekt pozwala przećwiczyć podstawowe i średniozaawansowane elementy SQL: SELECT, WHERE, ORDER BY, LIMIT, LIKE, GROUP BY, funkcje agregujące, JOIN, ROUND, COUNT(DISTINCT ...), podzapytania oraz funkcje okna. Jest również przykładem uporządkowania analizy danych w repozytorium GitHub.

