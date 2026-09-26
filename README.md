SQL Produkty i Sprzedaż 2025

Opis projektu

Projekt przedstawia przykładową bazę danych SQLite do nauki języka SQL i podstaw analizy danych sprzedażowych. Baza zawiera produkty z numerami EAN, kodami SKU, opisami i parametrami, a także klientów, dostawców, stany magazynowe, zamówienia i płatności.

Dane sprzedażowe obejmują rok 2025. Projekt pokazuje praktyczne wykorzystanie filtrowania, sortowania, agregacji, JOIN, funkcji tekstowych, obliczania marży oraz analizy sprzedaży według produktów, kategorii, kanałów i miesięcy.

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

Baza obejmuje 60 produktów, 400 klientów, 3 000 zamówień z 2025 roku, 7 522 pozycji zamówień, 6 kategorii, 6 dostawców oraz stany magazynowe w dwóch magazynach.

Pliki projektu

Plain Text


.
├── produkty_sprzedaz_2025.sqlite
├── produkty_sprzedaz_2025.sql
├── README.md
└── docs/
    └── screenshots/
        ├── 1.png
        ├── 2.png
        ├── ...
        └── 18.png



Plik .sqlite jest gotową bazą do wykonywania zapytań. Plik .sql jest tekstowym dumpem, który pozwala odtworzyć strukturę i dane w nowej bazie.

Uruchomienie w Windows

W PowerShellu przejdź do katalogu projektu:

Plain Text


cd "C:\Users\X\Downloads\sql-produkty-sprzedaz-2025"



Uruchom bazę:

Plain Text


C:\sqlite\sqlite3.exe ".\produkty_sprzedaz_2025.sqlite"



Po pojawieniu się promptu sqlite> wykonaj:

SQL


.headers on
.mode column
.tables



Przykładowe zapytanie:

SQL


SELECT produkt_id,
       ean,
       sku,
       nazwa,
       marka,
       cena_sprzedazy_netto
FROM produkty
LIMIT 5;


Najważniejsze tabele

Tabela
Opis
produkty
Produkty, EAN, SKU, ceny i parametry
kategorie
Kategorie produktów i stawki VAT
dostawcy
Dostawcy, kraje i oceny
klienci
Klienci B2C i B2B
zamowienia
Daty, kanały i statusy zamówień
pozycje_zamowien
Produkty i ilości w zamówieniach
platnosci
Informacje o płatnościach
stany_magazynowe
Ilości produktów w magazynach
v_sprzedaz_szczegoly
Widok ułatwiający analizę sprzedaży




Przykładowe raporty

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
GROUP BY kategoria
ORDER BY zysk_netto DESC;



Top 5 produktów według ilości

SQL


SELECT produkt,
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


SELECT kanal,
       COUNT(DISTINCT zamowienie_id) AS liczba_zamowien,
       SUM(ilosc) AS sprzedane_sztuki,
       ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto,
       ROUND(SUM(wartosc_brutto), 2) AS sprzedaz_brutto
FROM v_sprzedaz_szczegoly
WHERE status = 'Zrealizowane'
GROUP BY kanal
ORDER BY sprzedaz_netto DESC;



Dokumentacja wizualna

Poniższe screeny są przypisane zgodnie z rzeczywistą zawartością plików w folderze docs/screenshots.

1. Uruchomienie bazy i sprawdzenie katalogu

PowerShell przechodzi do katalogu projektu, wyświetla pliki, uruchamia SQLite i pokazuje prompt sqlite>.


![Screen 1](docs/screenshots/1.png)

2. Lista tabel

Ustawiono nagłówki i tryb kolumnowy, a następnie wykonano .tables. Wynik pokazuje tabele i widok dostępne w bazie.


![Screen 2](docs/screenshots/2.png)

3. Schemat tabeli produktów

Polecenie .schema produkty pokazuje definicję tabeli, klucz główny, klucze obce, ceny, EAN, SKU i ograniczenia danych.


![Screen 3](docs/screenshots/3.png)

4. Wybrane kolumny produktów

Zapytanie prezentuje wybrane informacje z tabeli produkty, w tym identyfikator, EAN, SKU, nazwę, opis i parametry.


![Screen 4](docs/screenshots/4.png)

5. Pełne rekordy produktów

Zapytanie SELECT * FROM produkty LIMIT 5 pokazuje pięć pełnych rekordów wraz ze wszystkimi kolumnami tabeli.


![Screen 5](docs/screenshots/5.png)

6. Filtrowanie produktów po cenie

Zapytanie wybiera aktywne produkty w przedziale cenowym od 50 do 150 zł i sortuje je według ceny malejąco.


![Screen 6](docs/screenshots/6.png)

7. Najdroższe produkty

Zapytanie wybiera 10 produktów o najwyższej cenie sprzedaży netto.


![Screen 7](docs/screenshots/7.png)

8. Wyszukiwanie tekstowe LIKE

Zapytanie wykorzystuje LIKE do znalezienia produktów, których nazwa zawiera słowo „bezprzewodowe”.


![Screen 8](docs/screenshots/8.png)

9. JOIN produktów, kategorii i dostawców

Zapytanie łączy tabele produkty, kategorie i dostawcy, pokazując kategorię, dostawcę, kraj oraz cenę produktu.


![Screen 9](docs/screenshots/9.png)

10. Sprzedaż i zysk według kategorii

Raport pokazuje sprzedaż netto, koszt zakupu, zysk netto i liczbę sprzedanych sztuk dla każdej kategorii.


![Screen 10](docs/screenshots/10.png)

11. Marża procentowa według kategorii

Raport oblicza marżę procentową jako udział zysku netto w sprzedaży netto.


![Screen 11](docs/screenshots/11.png)

12. Top 5 produktów według ilości

Raport pokazuje pięć produktów o największej liczbie sprzedanych sztuk. Drugim kryterium sortowania jest sprzedaż netto.


![Screen 12](docs/screenshots/12.png)

13. Pełny ranking produktów według ilości

To rozszerzona wersja rankingu bez LIMIT 5, pokazująca wszystkie produkty uporządkowane według sprzedanych sztuk.


![Screen 13](docs/screenshots/13.png)

14. Sprzedaż według kanałów dystrybucji

Raport porównuje liczbę zamówień, sprzedane sztuki, sprzedaż netto i sprzedaż brutto w kanałach dystrybucji.


![Screen 14](docs/screenshots/14.png)

15. Udział kanałów w sprzedaży

Zapytanie wykorzystuje funkcję okna do obliczenia procentowego udziału każdego kanału w całkowitej sprzedaży netto.


![Screen 15](docs/screenshots/15.png)

16. Sprzedaż miesięczna w 2025 roku

Raport grupuje sprzedaż po miesiącu i pokazuje liczbę zamówień, sprzedane sztuki oraz sprzedaż netto.


![Screen 16](docs/screenshots/16.png)

17. Najlepszy miesiąc sprzedażowy

Zapytanie wybiera miesiąc z najwyższą sprzedażą netto. W przedstawionym wyniku jest to listopad 2025.


![Screen 17](docs/screenshots/17.png)

18. Produkty poniżej progu magazynowego

Raport pokazuje SKU, nazwę produktu, magazyn, dostępną ilość i próg zamówienia dla produktów wymagających uzupełnienia.


![Screen 18](docs/screenshots/18.png)




Ćwiczenia SQL

1.
Wyświetl aktywne produkty marki Nova.

sqlite> SELECT
   ...>     produkt_id,
   ...>     ean,
   ...>     sku,
   ...>     nazwa,
   ...>     marka,
   ...>     cena_sprzedazy_netto,
   ...>     aktywny
   ...> FROM produkty
   ...> WHERE marka = 'Nova'
   ...>   AND aktywny = 1
   ...> ORDER BY cena_sprzedazy_netto DESC;
╭────────────┬────────────────┬──────────┬─────────────────────────────────┬───────┬──────────────────────┬─────────╮
│ produkt_id │      ean       │   sku    │              nazwa              │ marka │ cena_sprzedazy_netto │ aktywny │
╞════════════╪════════════════╪══════════╪═════════════════════════════════╪═══════╪══════════════════════╪═════════╡
│         40 │ '590202500040' │ SKU-0040 │ Krzesło biurowe Basic Nova 40   │ Nova  │                449.1 │       1 │
│         32 │ '590202500032' │ SKU-0032 │ Powerbank 20000 mAh Nova 32     │ Nova  │                149.0 │       1 │
│         24 │ '590202500024' │ SKU-0024 │ Lampka biurkowa LED Nova 24     │ Nova  │                130.9 │       1 │
│         16 │ '590202500016' │ SKU-0016 │ Mata do jogi Nova 16            │ Nova  │                75.05 │       1 │
│         56 │ '590202500056' │ SKU-0056 │ Mata do jogi Nova 56            │ Nova  │                75.05 │       1 │
│          8 │ '590202500008' │ SKU-0008 │ Zestaw herbat ziołowych Nova 8  │ Nova  │                47.25 │       1 │
│         48 │ '590202500048' │ SKU-0048 │ Zestaw herbat ziołowych Nova 48 │ Nova  │                47.25 │       1 │
╰────────────┴────────────────┴──────────┴─────────────────────────────────┴───────┴──────────────────────┴─────────╯

2.
Znajdź produkty w cenie netto od 50 do 150 zł.

sqlite> SELECT
   ...>     produkt_id,
   ...>     ean,
   ...>     sku,
   ...>     nazwa,
   ...>     marka,
   ...>     cena_sprzedazy_netto
   ...> FROM produkty
   ...> WHERE cena_sprzedazy_netto BETWEEN 50 AND 150
   ...> ORDER BY cena_sprzedazy_netto;
╭────────────┬────────────────┬──────────┬───────────────────────────────────────┬───────────┬──────────────────────╮
│ produkt_id │      ean       │   sku    │                 nazwa                 │   marka   │ cena_sprzedazy_netto │
╞════════════╪════════════════╪══════════╪═══════════════════════════════════════╪═══════════╪══════════════════════╡
│          5 │ '590202500005' │ SKU-0005 │ Butelka termiczna 750 ml ActiveX 5    │ ActiveX   │                 62.1 │
│         15 │ '590202500015' │ SKU-0015 │ Butelka termiczna 750 ml Orion 15     │ Orion     │                 62.1 │
│         25 │ '590202500025' │ SKU-0025 │ Butelka termiczna 750 ml HomeCraft 25 │ HomeCraft │                 62.1 │
│         35 │ '590202500035' │ SKU-0035 │ Butelka termiczna 750 ml OfficeOne 35 │ OfficeOne │                 62.1 │
│         45 │ '590202500045' │ SKU-0045 │ Butelka termiczna 750 ml ActiveX 45   │ ActiveX   │                 62.1 │
│         55 │ '590202500055' │ SKU-0055 │ Butelka termiczna 750 ml Orion 55     │ Orion     │                 62.1 │
│          6 │ '590202500006' │ SKU-0006 │ Mata do jogi Luma 6                   │ Luma      │                75.05 │
│         16 │ '590202500016' │ SKU-0016 │ Mata do jogi Nova 16                  │ Nova      │                75.05 │
│         26 │ '590202500026' │ SKU-0026 │ Mata do jogi GreenWay 26              │ GreenWay  │                75.05 │
│         36 │ '590202500036' │ SKU-0036 │ Mata do jogi PureLab 36               │ PureLab   │                75.05 │
│         46 │ '590202500046' │ SKU-0046 │ Mata do jogi Luma 46                  │ Luma      │                75.05 │
│         56 │ '590202500056' │ SKU-0056 │ Mata do jogi Nova 56                  │ Nova      │                75.05 │
│          3 │ '590202500003' │ SKU-0003 │ Mysz ergonomiczna OfficeOne 3         │ OfficeOne │                93.45 │
│         13 │ '590202500013' │ SKU-0013 │ Mysz ergonomiczna ActiveX 13          │ ActiveX   │                93.45 │
│         23 │ '590202500023' │ SKU-0023 │ Mysz ergonomiczna Orion 23            │ Orion     │                93.45 │
│         33 │ '590202500033' │ SKU-0033 │ Mysz ergonomiczna HomeCraft 33        │ HomeCraft │                93.45 │
│         43 │ '590202500043' │ SKU-0043 │ Mysz ergonomiczna OfficeOne 43        │ OfficeOne │                93.45 │
│         53 │ '590202500053' │ SKU-0053 │ Mysz ergonomiczna ActiveX 53          │ ActiveX   │                93.45 │
│          4 │ '590202500004' │ SKU-0004 │ Lampka biurkowa LED PureLab 4         │ PureLab   │                130.9 │
│         14 │ '590202500014' │ SKU-0014 │ Lampka biurkowa LED Luma 14           │ Luma      │                130.9 │
│         24 │ '590202500024' │ SKU-0024 │ Lampka biurkowa LED Nova 24           │ Nova      │                130.9 │
│         34 │ '590202500034' │ SKU-0034 │ Lampka biurkowa LED GreenWay 34       │ GreenWay  │                130.9 │
│         44 │ '590202500044' │ SKU-0044 │ Lampka biurkowa LED PureLab 44        │ PureLab   │                130.9 │
│         54 │ '590202500054' │ SKU-0054 │ Lampka biurkowa LED Luma 54           │ Luma      │                130.9 │
│          2 │ '590202500002' │ SKU-0002 │ Powerbank 20000 mAh GreenWay 2        │ GreenWay  │                149.0 │
│         12 │ '590202500012' │ SKU-0012 │ Powerbank 20000 mAh PureLab 12        │ PureLab   │                149.0 │
│         22 │ '590202500022' │ SKU-0022 │ Powerbank 20000 mAh Luma 22           │ Luma      │                149.0 │
│         32 │ '590202500032' │ SKU-0032 │ Powerbank 20000 mAh Nova 32           │ Nova      │                149.0 │
│         42 │ '590202500042' │ SKU-0042 │ Powerbank 20000 mAh GreenWay 42       │ GreenWay  │                149.0 │
│         52 │ '590202500052' │ SKU-0052 │ Powerbank 20000 mAh PureLab 52        │ PureLab   │                149.0 │
╰────────────┴────────────────┴──────────┴───────────────────────────────────────┴───────────┴──────────────────────╯


3.
Policz produkty w każdej kategorii.

sqlite> SELECT
   ...>     k.nazwa AS kategoria,
   ...>     COUNT(p.produkt_id) AS liczba_produktow
   ...> FROM kategorie AS k
   ...> LEFT JOIN produkty AS p
   ...>     ON p.kategoria_id = k.kategoria_id
   ...> GROUP BY k.kategoria_id, k.nazwa
   ...> ORDER BY liczba_produktow DESC;
╭─────────────────┬──────────────────╮
│    kategoria    │ liczba_produktow │
╞═════════════════╪══════════════════╡
│ Elektronika     │               18 │
│ Dom i ogród     │               12 │
│ Sport           │               12 │
│ Biuro           │                6 │
│ Zdrowie i uroda │                6 │
│ Żywność         │                6 │
╰─────────────────┴──────────────────╯

4.
Oblicz średnią cenę produktu według kategorii.

sqlite> SELECT
   ...>     k.nazwa AS kategoria,
   ...>     ROUND(AVG(p.cena_sprzedazy_netto), 2) AS srednia_cena_netto,
   ...>     ROUND(MIN(p.cena_sprzedazy_netto), 2) AS najnizsza_cena_netto,
   ...>     ROUND(MAX(p.cena_sprzedazy_netto), 2) AS najwyzsza_cena_netto
   ...> FROM kategorie AS k
   ...> LEFT JOIN produkty AS p
   ...>     ON p.kategoria_id = k.kategoria_id
   ...> GROUP BY k.kategoria_id, k.nazwa
   ...> ORDER BY srednia_cena_netto DESC;
╭─────────────────┬────────────────────┬──────────────────────┬──────────────────────╮
│    kategoria    │ srednia_cena_netto │ najnizsza_cena_netto │ najwyzsza_cena_netto │
╞═════════════════╪════════════════════╪══════════════════════╪══════════════════════╡
│ Dom i ogród     │              290.0 │                130.9 │                449.1 │
│ Elektronika     │              175.5 │                93.45 │               284.05 │
│ Sport           │              68.58 │                 62.1 │                75.05 │
│ Zdrowie i uroda │               49.0 │                 49.0 │                 49.0 │
│ Żywność         │              47.25 │                47.25 │                47.25 │
│ Biuro           │               31.9 │                 31.9 │                 31.9 │
╰─────────────────┴────────────────────┴──────────────────────┴──────────────────────╯

5.
Oblicz sprzedaż netto według miesięcy 2025 roku.

sqlite> SELECT
   ...>     strftime('%Y-%m', data_zamowienia) AS miesiac,
   ...>     ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto
   ...> FROM v_sprzedaz_szczegoly
   ...> WHERE status = 'Zrealizowane'
   ...>   AND data_zamowienia >= '2025-01-01'
   ...>   AND data_zamowienia < '2026-01-01'
   ...> GROUP BY strftime('%Y-%m', data_zamowienia)
   ...> ORDER BY miesiac;
╭─────────┬────────────────╮
│ miesiac │ sprzedaz_netto │
╞═════════╪════════════════╡
│ 2025-01 │      129552.14 │
│ 2025-02 │      109840.08 │
│ 2025-03 │      125205.22 │
│ 2025-04 │      110970.72 │
│ 2025-05 │      133404.97 │
│ 2025-06 │      110594.32 │
│ 2025-07 │      134552.38 │
│ 2025-08 │      115487.94 │
│ 2025-09 │      100901.02 │
│ 2025-10 │      128051.48 │
│ 2025-11 │       162273.6 │
│ 2025-12 │      119079.02 │
╰─────────┴────────────────╯
sqlite> 

6.
Znajdź pięć produktów z największą liczbą sprzedanych sztuk.

sqlite> SELECT
   ...>     produkt,
   ...>     ean,
   ...>     sku,
   ...>     SUM(ilosc) AS sprzedane_sztuki,
   ...>     ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto
   ...> FROM v_sprzedaz_szczegoly
   ...> WHERE status = 'Zrealizowane'
   ...>   AND data_zamowienia >= '2025-01-01'
   ...>   AND data_zamowienia < '2026-01-01'
   ...> GROUP BY produkt_id, produkt, ean, sku
   ...> ORDER BY sprzedane_sztuki DESC
   ...> LIMIT 5;
╭───────────────────────────────────┬────────────────┬──────────┬──────────────────┬────────────────╮
│              produkt              │      ean       │   sku    │ sprzedane_sztuki │ sprzedaz_netto │
╞═══════════════════════════════════╪════════════════╪══════════╪══════════════════╪════════════════╡
│ Mysz ergonomiczna ActiveX 53      │ '590202500053' │ SKU-0053 │              234 │       21021.71 │
│ Butelka termiczna 750 ml Orion 55 │ '590202500055' │ SKU-0055 │              233 │       14031.45 │
│ Krzesło biurowe Basic GreenWay 10 │ '590202500010' │ SKU-0010 │              225 │       98150.75 │
│ Butelka termiczna 750 ml Orion 15 │ '590202500015' │ SKU-0015 │              225 │       13596.68 │
│ Krem nawilżający SPF30 ActiveX 37 │ '590202500037' │ SKU-0037 │              223 │        10559.5 │
╰───────────────────────────────────┴────────────────┴──────────┴──────────────────┴────────────────╯
sqlite> 

7.
Oblicz zysk i marżę według kategorii.

sqlite> SELECT
   ...>     kategoria,
   ...>     ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto,
   ...>     ROUND(SUM(cena_zakupu_netto * ilosc), 2) AS koszt_zakupu,
   ...>     ROUND(
(x1...>         SUM(wartosc_netto)
(x1...>         - SUM(cena_zakupu_netto * ilosc),
(x1...>         2
(x1...>     ) AS zysk_netto,
   ...>     ROUND(
(x1...>         100.0 * (
(x2...>             SUM(wartosc_netto)
(x2...>             - SUM(cena_zakupu_netto * ilosc)
(x2...>         ) / NULLIF(SUM(wartosc_netto), 0),
(x1...>         2
(x1...>     ) AS marza_procent
   ...> FROM v_sprzedaz_szczegoly
   ...> WHERE status = 'Zrealizowane'
   ...>   AND data_zamowienia >= '2025-01-01'
   ...>   AND data_zamowienia < '2026-01-01'
   ...> GROUP BY kategoria
   ...> ORDER BY zysk_netto DESC;
╭─────────────────┬────────────────┬──────────────┬────────────┬───────────────╮
│    kategoria    │ sprzedaz_netto │ koszt_zakupu │ zysk_netto │ marza_procent │
╞═════════════════╪════════════════╪══════════════╪════════════╪═══════════════╡
│ Dom i ogród     │      642630.56 │    345903.75 │  296726.81 │         46.17 │
│ Elektronika     │      548401.35 │    296800.95 │   251600.4 │         45.88 │
│ Sport           │       144351.9 │     73979.65 │   70372.25 │         48.75 │
│ Zdrowie i uroda │        58417.8 │     23114.45 │   35303.35 │         60.43 │
│ Żywność         │       49472.88 │      25146.0 │   24326.88 │         49.17 │
│ Biuro           │        36638.4 │      13905.0 │    22733.4 │         62.05 │
╰─────────────────┴────────────────┴──────────────┴────────────┴───────────────╯
sqlite> 

8.
Porównaj sprzedaż klientów B2C i B2B.

sqlite> SELECT
   ...>     typ_klienta,
   ...>     COUNT(DISTINCT zamowienie_id) AS liczba_zamowien,
   ...>     COUNT(DISTINCT klient_id) AS liczba_klientow,
   ...>     SUM(ilosc) AS sprzedane_sztuki,
   ...>     ROUND(SUM(wartosc_netto), 2) AS sprzedaz_netto,
   ...>     ROUND(
(x1...>         SUM(wartosc_netto)
(x1...>         / NULLIF(COUNT(DISTINCT zamowienie_id), 0),
(x1...>         2
(x1...>     ) AS srednia_wartosc_zamowienia
   ...> FROM v_sprzedaz_szczegoly
   ...> WHERE status = 'Zrealizowane'
   ...>   AND data_zamowienia >= '2025-01-01'
   ...>   AND data_zamowienia < '2026-01-01'
   ...> GROUP BY typ_klienta
   ...> ORDER BY sprzedaz_netto DESC;
╭─────────────┬─────────────────┬─────────────────┬──────────────────┬────────────────┬──────────────────────╮
│ typ_klienta │ liczba_zamowien │ liczba_klientow │ sprzedane_sztuki │ sprzedaz_netto │ srednia_wartosc_z... │
╞═════════════╪═════════════════╪═════════════════╪══════════════════╪════════════════╪══════════════════════╡
│ B2C         │            2078 │             356 │             9968 │     1332369.34 │               641.18 │
│ B2B         │             260 │              44 │             1165 │      147543.55 │               567.48 │
╰─────────────┴─────────────────┴─────────────────┴──────────────────┴────────────────┴──────────────────────╯
sqlite> 

9.
Znajdź produkty poniżej progu zamówienia w magazynie.

sqlite> SELECT
   ...>     p.produkt_id,
   ...>     p.ean,
   ...>     p.sku,
   ...>     p.nazwa,
   ...>     s.magazyn,
   ...>     s.ilosc_dostepna,
   ...>     s.prog_zamowienia,
   ...>     s.prog_zamowienia - s.ilosc_dostepna AS brakujaca_ilosc
   ...> FROM produkty AS p
   ...> JOIN stany_magazynowe AS s
   ...>     ON s.produkt_id = p.produkt_id
   ...> WHERE s.ilosc_dostepna < s.prog_zamowienia
   ...> ORDER BY brakujaca_ilosc DESC;
╭────────────┬────────────────┬──────────┬───────────────────────────────────────┬──────────┬────────────────┬─────────────────┬─────────────────╮
│ produkt_id │      ean       │   sku    │                 nazwa                 │ magazyn  │ ilosc_dostepna │ prog_zamowienia │ brakujaca_ilosc │
╞════════════╪════════════════╪══════════╪═══════════════════════════════════════╪══════════╪════════════════╪═════════════════╪═════════════════╡
│         28 │ '590202500028' │ SKU-0028 │ Zestaw herbat ziołowych PureLab 28    │ Poznań   │              0 │              21 │              21 │
│          7 │ '590202500007' │ SKU-0007 │ Krem nawilżający SPF30 Orion 7        │ Warszawa │              7 │              27 │              20 │
│         33 │ '590202500033' │ SKU-0033 │ Mysz ergonomiczna HomeCraft 33        │ Poznań   │              6 │              26 │              20 │
│          6 │ '590202500006' │ SKU-0006 │ Mata do jogi Luma 6                   │ Poznań   │             12 │              29 │              17 │
│         35 │ '590202500035' │ SKU-0035 │ Butelka termiczna 750 ml OfficeOne 35 │ Warszawa │              8 │              16 │               8 │
│          4 │ '590202500004' │ SKU-0004 │ Lampka biurkowa LED PureLab 4         │ Warszawa │             16 │              22 │               6 │
│         16 │ '590202500016' │ SKU-0016 │ Mata do jogi Nova 16                  │ Warszawa │             22 │              28 │               6 │
│         14 │ '590202500014' │ SKU-0014 │ Lampka biurkowa LED Luma 14           │ Poznań   │              8 │              13 │               5 │
│         38 │ '590202500038' │ SKU-0038 │ Zestaw herbat ziołowych Luma 38       │ Poznań   │             24 │              28 │               4 │
│          5 │ '590202500005' │ SKU-0005 │ Butelka termiczna 750 ml ActiveX 5    │ Warszawa │             11 │              13 │               2 │
╰────────────┴────────────────┴──────────┴───────────────────────────────────────┴──────────┴────────────────┴─────────────────┴─────────────────╯
sqlite> 

10.
Oblicz udział kanałów dystrybucji w sprzedaży netto.

sqlite> WITH sprzedaz_kanalow AS (
(x1...>     SELECT
(x1...>         kanal,
(x1...>         SUM(wartosc_netto) AS sprzedaz_netto
(x1...>     FROM v_sprzedaz_szczegoly
(x1...>     WHERE status = 'Zrealizowane'
(x1...>       AND data_zamowienia >= '2025-01-01'
(x1...>       AND data_zamowienia < '2026-01-01'
(x1...>     GROUP BY kanal
(x1...> )
   ...> SELECT
   ...>     kanal,
   ...>     ROUND(sprzedaz_netto, 2) AS sprzedaz_netto,
   ...>     ROUND(
(x1...>         100.0 * sprzedaz_netto
(x1...>         / SUM(sprzedaz_netto) OVER (),
(x1...>         2
(x1...>     ) AS udzial_procent
   ...> FROM sprzedaz_kanalow
   ...> ORDER BY sprzedaz_netto DESC;
╭───────────────────┬────────────────┬────────────────╮
│       kanal       │ sprzedaz_netto │ udzial_procent │
╞═══════════════════╪════════════════╪════════════════╡
│ Sklep online      │      880674.55 │          59.51 │
│ Marketplace       │      300460.66 │           20.3 │
│ Sklep stacjonarny │      213435.57 │          14.42 │
│ Telefon           │       85342.11 │           5.77 │
╰───────────────────┴────────────────┴────────────────╯
sqlite> 


Status projektu

Projekt zawiera gotową bazę SQLite, dump SQL, dokumentację uruchomienia, przykładowe zapytania oraz screeny pokazujących kolejne etapy pracy z bazą.

