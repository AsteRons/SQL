TWORZENIE TWBLICY:

CREATE TABLE nazwa
(

nazwa_kolumny  char(10) ,
.
.
.

);


DEFINIOWANIE KLUCZY:

ALTER TABLE Klienci ADD PRIMARY KEY (kl_id);


1. POBIERANIE DANYCH

						SELECT


SELECT * FROM Produkty;

SELECT prod_id, prod_nazwa FROM Produkty;

SELECT DISTINCT dost_id FROM Produkty;											-	nie powtarza siê

SELECT prod_nazwa FROM Produkty FETCH FIRST 5 ROWS ONLY;			-	5 górnych

lub 

SELECT prod_nazwa FROM Produkty LIMIT 5;

-- komentarz 																					- komentarz
			
			
			
			
			
						SORTOWANIE							ORDER BY
						
						
SELECT prod_nazwa FROM Produkty ORDER BY prod_nazwa;		-posortowana od A do Z

SELECT prod_nazwa FROM Produkty ORDER BY prod_nazwa DESC;		-posortowana od Z do A


SELECT prod_nazwa FROM Produkty ORDER BY 1;		- wedlug polozenia kolumny



						
				FILTROWANIE DANYCH 								WHERE
				
	
SELECT prod_nazwa, prod_cena FROM Produkty WHERE prod_cena = 9.99;			-		pobiuera dane z tabeli gdzie prod = 9.99

SELECT prod_nazwa, prod_cena FROM Produkty WHERE prod_cena BETWEEN 10 AND 30;




			ZAAWANSOWANE SOEROWANIE 			WHERE												 NOT, IN
			
Stosowanie wyrazów OR lub AND


SELECT prod_id, prod_cena, prod_nazwa FROM Produkty WHERE dost_id = 'DLL01' AND prod_cena <= 10;



Operator IN - umo¿liwia okreœlenie zakresu warunków, z których dowolny moze byc spelniony.


SELECT prod_nazwa, prod_cena FROM Produkty WHERE dost_id IN ('DLL01', 'BRS01') ORDER BY prod_nazwa;


Operator NOT	-	neguje dowolny warunek

SELECT prod_nazwa, prod_cena FROM Produkty WHERE NOT dost_id = 'DLL01' ORDER BY prod_nazwa;



		FILTROWANIE ZA POMOC¥ ZNAKÓW WIELOZNACZNYCH			LIKE
		
		
		Like oraz %
		
SELECT prod_id, prod_nazwa FROM Produkty WHERE prod_nazwa LIKE 'Rybka%';	-	wyszukuje wartoœci zawieraj¹c¹ fraze 'Rybka'
		
		
		
		TWORZENIE PÓL OBLICZENIOWYCH:
		
SELECT dost_nazwa || ' (' || dost_kraj || ')' FROM Dostawcy ORDER BY dost_nazwa;					- || oznacza to samo co + w Stringu

SELECT RTRIM(dost_nazwa) || ' (' || RTRIM(dost_kraj) || ')' AS dost_tytul FROM Dostawcy ORDER BY dost_nazwa;			- dodawanie nazwy tabeli


			MODYWIKOWANIE DANYCH ZA POMOC¥ FUNKCJI
			
	
RTRIM()	-	usuwa spacje na koncu kolumny
UPPER()	-	zamienia na duze litery
LENGTH()	-	zwraca dlugosc
LOWER()	-	zwraca na male litery


SELECT * FROM Zamowienia WHERE DATE_PART('year', zam_data) = 2012;	-	zwraca zamówienia z roku 2012



			FUNKCJE AGREGUJ¥CE:
			
AVG()		 zwraca œredni¹ wartoœæ kolumny
COUNT() 	 zwraca liczbê wierszy w kolumnie
MAX() 		 zwraca najwiêksz¹ wartoœæ w kolumnie
MIN() 		 zwraca najmniejsz¹ wartoœæ w kolumnie
SUM() 		 zwraca sumê wartoœci w kolumnie
			
			
			
SELECT AVG(DISTINCT prod_cena) AS sr_cena FROM Produkty WHERE dost_id = 'DLL01';
			
		
SELECT COUNT(*) AS liczba_elementow,
MIN(prod_cena) AS cena_min,
MAX(prod_cena) AS cena_max,
AVG(prod_cena) AS cena_sr
FROM Produkty;
			
			

				GRUPOWANIE DANYCH

SELECT dost_id, COUNT(*) AS liczba_prod FROM Produkty GROUP BY dost_id;					-		grupuje je i sortuje po dost_id		

SELECT kl_id, COUNT(*) AS zamowienia	FROM Zamowienia	GROUP BY kl_id	HAVING COUNT(*) >= 2;	-	filtrowanie grupy



					ZAPYTANIA ZAGNIE¯D¯ONE
					
	SELECT kl_nazwa, kl_kontakt
FROM Klienci
WHERE kl_id IN (SELECT kl_id
FROM Zamowienia
WHERE zam_numer IN (SELECT zam_numer
FROM ElementyZamowienia
WHERE prod_id = 'RGAN01'));
	

	
					£¥CZENIE TABEL
					
SELECT dost_nazwa, prod_nazwa, prod_cena FROM Dostawcy, Produkty WHERE Dostawcy.dost_id = Produkty.dost_id;

SELECT dost_nazwa, prod_nazwa, prod_cena FROM Dostawcy INNER JOIN Produkty ON Dostawcy.dost_id = Produkty.dost_id;



			
			TWORZENIE ROZBUDOWANYCH ZA£¥CZEÑ
			
			Stosowanie Aliasingów do tabel:
			
			
SELECT kl_nazwa, kl_kontakt
FROM Klienci AS K, Zamowienia AS Z, ElementyZamowienia AS E
WHERE K.kl_id = Z.kl_id
AND E.zam_numer = Z.zam_numer
AND prod_id = 'RGAN01';
			
			
SELECT Klienci.kl_id, Zamowienia.zam_numer
FROM Klienci INNER JOIN Zamowienia
ON Klienci.kl_id = Zamowienia.kl_id;	
			
			
SELECT Klienci.kl_id, Zamowienia.zam_numer
FROM Klienci RIGHT OUTER JOIN Zamowienia
ON Zamowienia.kl_id = Klienci.kl_id;



			£¥CZENIE ZAPYTAÑ
			
			
SELECT kl_nazwa, kl_kontakt, kl_email
FROM Klienci
WHERE kl_woj IN ('MAL', 'MAZ', 'WKP')
UNION
SELECT kl_nazwa, kl_kontakt, kl_email
FROM Klienci
WHERE kl_nazwa = 'Zabawa dla wszystkich';



			WSTAWIANIE DANYCH			INSERT
			
INSERT - wprowadzanie danych do bazy

-	wstawianie pe³nego wiersza
-	wstawianie jednego wiersza
-	 wstawianie wyników zapytania
			
			
INSERT INTO Klienci
VALUES('1000000006',
'Zabawkolandia'
'Miodowa 12',
'Katowice'
'SLK',
'41-200',
'Polska',
NULL,
NULL);


INSERT INTO Klienci(kl_id,
kl_kontakt,
kl_email,
kl_nazwa,
kl_adres,
kl_miasto,
kl_woj,
kl_kod,
kl_kraj)
VALUES('1000000006',
NULL,
NULL,
'Zabawkolandia'
'Miodowa 12',
'Katowice'
'SLK',
'41-200',
'Polska');


INSERT INTO Klienci(kl_id,
kl_kontakt,
kl_email,
kl_nazwa,
kl_adres,
kl_miasto,
kl_woj,
kl_kod,
kl_kraj)
SELECT kl_id,
kl_kontakt,
kl_email,
kl_nazwa,
kl_adres,
kl_miasto,
kl_woj,
kl_kod,
kl_kraj
FROM KlienciNowi;


Kopiowanie z jednej do drugiej

SELECT *
INTO KopiaKlienci
FROM Klienci;


CREATE TABLE KopiaKlienci AS
SELECT * FROM Klienci;



				AKTUALIZACJA I USUWANIE DANYCH:
	

USTAWIANIE DANYCH:
	
UPDATE Klienci
SET kl_email = 'ms@skladzabawek.pl'
WHERE kl_id = '1000000005';


UPDATE Klienci
SET kl_email =NULL
WHERE kl_id = '1000000005';

USUWANIE DANYCH:

DELETE FROM Klienci
WHERE kl_id = '1000000006';



		TWORZENIE I MODYFIKOWANIE TABEL:
	
	
	CREATE TABLE ElementyZamowienia
(
zam_numer INTEGER NOT NULL,
zam_element INTEGER NOT NULL,
prod_id CHAR(10) NOT NULL,
ilosc INTEGER NOT NULL DEFAULT 1,
cena_elem DECIMAL(8,2) NOT NULL
);	

DODANIE 	KOLUMNY
	
ALTER TABLE Dostawcy
ADD dost_telefon CHAR(20);

	USUWANIE TABELI:
	
DROP TABLE KopiaKlienci;



		STOSOWANIE PERSPEKTYW
		


CREATE VIEW ProduktyKlientow AS
SELECT kl_nazwa, kl_kontakt, prod_id
FROM Klienci, Zamowienia, ElementyZamowienia
WHERE Klienci.kl_id = Zamowienia.kl_id
AND ElementyZamowienia.zam_numer = Zamowienia.zam_numer;


		
SELECT kl_nazwa, kl_kontakt
FROM ProduktyKlientow
WHERE prod_id = 'RGAN01';


CREATE VIEW LokalizacjeKlientow AS
SELECT RTRIM(dost_nazwa) + ' (' + RTRIM(dost_kraj) + ')' AS dost_tytul
FROM Dostawcy
ORDER BY dost_nazwa;

CREATE VIEW WartosciElementowZamowienia
SELECT zam_numer,
prod_id,
ilosc,
cena_elem,
ilosc*cena_elem AS wartosc
FROM ElementyZamowienia;




			KORZYSTANIE Z ZAPAMIETANYCH PROCEDURE
			
			
			
			CREATE PROCEDURE ZliczanieListyMailingowej(
RozmiarListy OUT NUMBER
)
IS
l_wierszy INTEGER;
BEGIN
SELECT COUNT(*) INTO l_wierszy
FROM Klienci
WHERE NOT kl_email IS NULL;
RozmiarListy := l_wierszy;
END;


