# Rekordok státuszai, listázása, adatlapja
### Rekordok státuszai
Egy rekord (kép, tárgy, dokumentum) a létrehozás (feltöltés) során több státuszon megy keresztül:
1. létrehozás -> létrejön a rekord ID-ja
2. feltöltés -> ez egy köztes státusz, amikor a rekord adatai feltöltésre kerülnek, azonban **nem** az összes -> a feltöltés a **mentés gomb** megnyomásával zárul
	1. a feltöltést bárki végezheti, azonban
	2. feltöltést, tehát rekordot csak erre jogosult felhasználó véglegesíthet
3.  feltöltés véglegesítése -> az erre jogosult felhasználó fejezi be a feltöltést (például meghatározza a rekord tárgyának fizikai helyét (dobozID), valamint ellenőrzi a felvitt adatokat) -> a feltöltés a **rekord véglegesítése váltókapcsoló** átállításával és a **mentés gomb** megnyomásával zárul (a váltókapcsoló csak az arra jogosult felhasználók számra aktív)
4. törölt -> speciális státusz. 
	1. ebben az állapotban **nem** kerül fizikailag törlésre a rekord, hanem kap egy törölt flaget, és átkerül a törölt elemek listába
	2. a **törlés gombot** csak az erre jogosult felhasználók nyomhatják meg (a gomb biztonsági kérdést jelenít meg: biztos/mégsem)

### Rekordok listázása
A leltárprogram adminisztrációs felületén külön listák segítik a feltöltést végző felhasználókat:
	1. befejezett feltöltések -> itt a véglegesített rekordokat listázzuk
	2. nyitott feltöltések -> a véglegesítésre váró feltöltések
	3. törölt feltöltések

### Funkciók a rekordok listájában
A rekordokat listázó felületen a felhasználók számára a következő lehetőségeket kell biztosítani:
	1. rekord egyértelmű beazonosítása legalább a következő elemekkel:
		1. ID
		2. rekord típusa (kép, tárgy, dokumentum)
		3. rekord neve (a megnevezés)
		4. rekord helye (dobozID)
		5. rekord pozíciója (vitrinID)
	2. szerkesztés gomb -> a rekord feltöltési/szerkesztési oldalára visz
	3. rekord adatlapja -> a rekord adatlapjára viszi
	4. táblázatfejlécben rendezési lehetőség
	5. az adminisztrációs lapon keresési lehetőség (szabadszavas)

### Rekordok adatlapja


#munka/Futball Ház/leltár/tartalom/rekord státuszai#