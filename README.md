
zad 1.
SELECT *
FROM klienci
WHERE miasto = 'Lublin';

zad 2.
SELECT tytul, Cena
FROM ksiazki
WHERE Cena > 40;

zad 3.
SELECT miasto, COUNT(miasto) AS liczba_klientow
FROM klienci
GROUP BY miasto;

zad 4.
SELECT id_klienta, COUNT(*) 
FROM sprzedaz
GROUP BY id_klienta;

zad 5.
SELECT *
FROM klienci
WHERE id_klienta NOT IN (
    SELECT id_klienta
    FROM sprzedaz
);

zad 6.
SELECT DISTINCT k.*
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
JOIN gatunki g ON ks.id_gatunku = g.id_gatunku
WHERE g.gatunek = 'Fantastyka';

zad 7.
SELECT g.gatunek, AVG(ks.Cena) AS srednia_cena
FROM gatunki g
JOIN ksiazki ks ON g.id_gatunku = ks.id_gatunku
GROUP BY g.id_gatunku, g.gatunek;

zad 8.
SELECT ks.tytul
FROM ksiazki ks
LEFT JOIN sprzedaz s ON ks.id_ksiazki = s.id_ksiazki
WHERE s.id_sprzedazy IS NULL;

zad 9. 
SELECT k.id_klienta, k.imie, k.nazwisko,
       COUNT(s.id_sprzedazy) AS ilosc_ksiazek
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
GROUP BY k.id_klienta, k.imie, k.nazwisko
HAVING COUNT(s.id_sprzedazy) > 3;

zad 10.
SELECT w.wydawnictwo,
       COUNT(ks.id_ksiazki) AS ilosc_ksiazek
FROM wydawnictwa w
LEFT JOIN ksiazki ks
    ON w.id_wydawnictwa = ks.id_wydawnictwa
GROUP BY w.id_wydawnictwa, w.wydawnictwo;

zad 11.
SELECT DISTINCT k.*
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
JOIN gatunki g ON ks.id_gatunku = g.id_gatunku
WHERE g.gatunek IN ('Sensacja', 'Thriller');

zad 12.
SELECT DISTINCT k.*
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
JOIN wydawnictwa w ON ks.id_wydawnictwa = w.id_wydawnictwa
WHERE w.wydawnictwo = 'Helion';

zad 13.
SELECT g.gatunek,
       COUNT(ks.id_ksiazki) AS ilosc_ksiazek
FROM gatunki g
LEFT JOIN ksiazki ks
    ON g.id_gatunku = ks.id_gatunku
GROUP BY g.id_gatunku, g.gatunek;

zad 14. 
SELECT ks.tytul,
       COUNT(s.id_sprzedazy) AS ilosc_sprzedazy
FROM ksiazki ks
JOIN sprzedaz s ON ks.id_ksiazki = s.id_ksiazki
GROUP BY ks.id_ksiazki, ks.tytul
HAVING COUNT(s.id_sprzedazy) > 2;

zad 15.
SELECT p.imie, p.nazwisko, s.nazwa AS stanowisko
FROM pracownicy p
JOIN stanowiska s
    ON p.id_stanowiska = s.id_stanowiska
WHERE p.wynagrodzenie > (
    SELECT AVG(wynagrodzenie)
    FROM pracownicy
);

zad 16.
SELECT DISTINCT ks.tytul
FROM ksiazki ks
JOIN sprzedaz s ON ks.id_ksiazki = s.id_ksiazki
JOIN klienci k ON s.id_klienta = k.id_klienta
WHERE k.nazwisko = 'Kowalski';

zad 17.
SELECT k.id_klienta, k.imie, k.nazwisko,
       SUM(ks.Cena) AS laczna_wartosc
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
GROUP BY k.id_klienta, k.imie, k.nazwisko
HAVING SUM(ks.Cena) > 100;

zad 18.
SELECT s.nazwa AS stanowisko,
       COUNT(p.id_pracownika) AS liczba_pracownikow
FROM stanowiska s
LEFT JOIN pracownicy p
    ON s.id_stanowiska = p.id_stanowiska
GROUP BY s.id_stanowiska, s.nazwa;

zad 19.
SELECT k.id_klienta, k.imie, k.nazwisko,
       COUNT(s.id_sprzedazy) AS ilosc_zakupow
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
GROUP BY k.id_klienta, k.imie, k.nazwisko
ORDER BY ilosc_zakupow DESC
LIMIT 1;

zad 20.
SELECT k.id_klienta, k.imie, k.nazwisko
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
JOIN gatunki g ON ks.id_gatunku = g.id_gatunku
WHERE g.gatunek IN ('Fantastyka', 'Sensacja')
GROUP BY k.id_klienta, k.imie, k.nazwisko
HAVING COUNT(DISTINCT g.gatunek) = 2;

zad 21.
SELECT k.id_klienta, k.imie, k.nazwisko,
       SUM(ks.Cena) AS laczna_wartosc
FROM klienci k
JOIN sprzedaz s ON k.id_klienta = s.id_klienta
JOIN ksiazki ks ON s.id_ksiazki = ks.id_ksiazki
GROUP BY k.id_klienta, k.imie, k.nazwisko
HAVING SUM(ks.Cena) > (
    SELECT AVG(wartosc)
    FROM (
        SELECT SUM(ks2.Cena) AS wartosc
        FROM sprzedaz s2
        JOIN ksiazki ks2
            ON s2.id_ksiazki = ks2.id_ksiazki
        GROUP BY s2.id_klienta
    ) AS srednie_zakupy
);
