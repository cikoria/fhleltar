# Futball Ház leltárprogram specifikáció 
## Döntést igénylő kérdések
- a képeslapok képek vagy dokumentumok?
- - - -
## Tartalomjegyzék
- Általános leírás
	- Szószedet
- Keretrendszer
	- Admin felület
	- Felhasználók, jogkezelés
	- Rekordok státuszai, listázása, adatlapja
- Adattípusok
	- Közös blokk
	- Típus: kép
	- Típus: tárgy
	- Típus: dokumentum

- - - -
## Általános leírás
A Futball Ház leltárprogram egy egyedi igényekre szabott adatbázis-keretrendszer. A leltárprogram elsődleges funkciója a Kispesti Futball Ház állományában lévő tárgyak nyilvántartása. A leltárprogram másodlagos funkciója ezen tárgyak publikálásának, szélesebb nyilvánosság felé történő bemutatásának elősegítése.

### Szószedet
- rekord: a leltár egy eleme, ami lehet kép, tárgy vagy dokumentum
- doboz: az adott leltári elem fizikai helye, ha a raktárban van
- vitrin: az adott leltári elem fizikai helye, ha a kiállítótérben van
- adatlap: az adott rekordhoz tartozó tárgyat bemutató oldal

- - - -
## Admin felület
A leltárprogram elsődleges felülete az admin felület.
A felhasználók ide jelentkeznek be, itt töltik fel és adminisztrálják a rekordokat.

Az admin felület legfontosabb funkciói:
- autentikáció (bejelentkezés, kijelentkezés)
- saját profil módosítása -> jelszómódosítás
- rekordok létrehozása
- rekordok listázása
- rekordok szerkesztése
- [opcionális] felhasználókezelés

## Felhasználók, jogkezelés
### Általános leírás
**Nem fontos ehhez admin frontendet írni, bőven elég kézzel editálni a usereket.**

A felhasználók email-címmel és jelszóval jelentkeznek be. A rendszer a felhasználót alapértelmezett jelszóval hozza létre, majd a felhasználók azt az admin felületen megváltoztatják. Kerüljük el az `Almafa123` típusú jelszavakat.

### Felhasználói kategóriák és jogosultságaik
- adminisztrátor
	- bejelentkezés
	- [opcionális] admin frontend esetén: felhasználó létrehozása és jogosultságának megadása
	- rekord létrehozása, feltöltés megkezdése, mentése
	- rekord végelegesítése
	- véglegesített rekord státuszának megváltoztatása nem véglegesre
	- rekord törlése
	- véglegesített, **publikus**, **kutatható** és **zárt** rekordok listázása
- feltöltő
	- bejelentkezés
	- rekord létrehozása, feltöltés megkezdése, mentése
	- nem véglegesített rekordok szerkesztése
	- véglegesített, **publikus**, **kutatható** és **zárt** rekordok listázása
- kutató
	- bejelentkezés
	- véglegesített, **publikus** és **kutatható** rekordok listázása
- látogató
	- bejelentkezés
	- véglegesített és **publikus** rekordok listázása

## Rekordok státuszai, listázása, adatlapja
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

### Rekord láthatósága
Az egyes rekordoknak három láthatósági állapota létezik, amikre **kizárólag** az adminisztrátor jogkörrel rendelkező felhasználók állíthatnak be (a feltöltők számára inaktív mező).
1. zárt -> 
2. kutatható ->
3. publikus ->

### Rekordok listázása
A leltárprogram adminisztrációs felületén külön listák segítik a feltöltést végző felhasználókat:
1. befejezett feltöltések -> itt a véglegesített rekordokat listázzuk
2. nyitott feltöltések -> a véglegesítésre váró feltöltések
3. törölt feltöltések

### Funkciók a rekordok listájában
A rekordokat listázó felületen a felhasználók számára a következő lehetőségeket kell biztosítani:
1. új rekord hozzáadása -> egyetlen kérdés: a rekord típusa (kép, tárgy, dokumentum)
2. rekord egyértelmű beazonosítása legalább a következő elemekkel:
	1. ID
	2. rekord típusa (kép, tárgy, dokumentum)
	3. rekord neve (a megnevezés)
	4. rekord helye (dobozID)
	5. rekord pozíciója (vitrinID)
	6. rekord láthatósága
3. szerkesztés gomb -> a rekord feltöltési/szerkesztési oldalára visz
4. rekord adatlapja -> a rekord adatlapjára viszi
5. táblázatfejlécben rendezési lehetőség
6. az adminisztrációs lapon keresési lehetőség (szabadszavas)

### Rekordok adatlapja
A rekord adatlapja egy egyedi stíluslappal ellátott oldal, ahol az adott rekord adatait meg lehet tekinteni. -> A későbbi publikálás alapját ezek az oldalak képezik.

A rekordok adatlapját a rekordok listájából lehet elérni. Az egyes rekordok adatlapjaihoz csak az arra jogosult felhasználók férhetnek hozzá.

A rekordok adatlapján **csak** azok az adatok kerülnek listázásra, amelyek közvetlenül a rekordhoz köthetők, tehát
- a rekordot adminisztráló felhasználók adatai nem
- a rekordhoz tartozó státusz és egyéb információk nem
- a rekord fizikai helyéből csak a vitrin azonosítója.
	- **ha** egy rekord tárgya nincs vitrinben, akkor `raktárban` felirat olvasható helyette.
- [opcionális] nyomtatási kép -> a rekord adatlapját böngésző felhasználó másolatot készíthet magának (PDF exportálás)

- - - -
## Adattípusok
### Közös blokk
A közös blokk minden adattípusnál fixen jelen van a rekord felvitele és szerkesztése oldalon.
- rekord láthatóságának beállítása
- rekord véglegesítése váltókapcsoló
- mentés
- törlés

### Közös blokk / előzmények
- [opcionális] a rekordoknál megjelenítésre kerül a rekord története:
	- ki hozta létre a rekordot és mikor
	- ki szerkesztette utoljára a rekordot és mikor
	- [opcionális] a szerkesztési előzmények listázása -> user és dátum

## Típus: kép
- **ID**: ::formátumát még ki kell találni::
- **kép megnevezése**: szöveg
- **kép**: bináris, maga a képállomány
- **hátlap**: bináris, képállomány (ha a hátlapon van valami)
	- [opcionális] **kép felbontása**: ha ki tudod olvasni, akkor tároljuk
- **kép színei**: színes / hamisszínes / festett / fekete-fehér / egyéb
- **kép helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a képet) -> ezeket az ID-kat mi határozzuk majd meg!
- **kép pozíciója**:
	- dobozban: igen/nem
	- kölcsönben: szöveg (hova adtuk kölcsön)
	- vitrinben: vitrinID
- **kép forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsönkép**: igen/nem
	- **ha igen**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **hivatkozás**: URL (ha a kép a neten is elérhető)
- **kép mérete**: kicsi / közepes / nagy (szemre határozzuk meg)
- **kép darabszáma**: szám (mennyi van belőle)
- **kép típusa**: meccs / csapatkép / portré / képeslap / egyéb
- **kép helyszíne**: szöveg (egyelőre!, a kép készítésének helye, majd később lehet, ebből is rendezett lista lesz)
- **kép készítésének ideje**: 
	- bizonytalan: igen/nem
		- **ha** igen: szöveg (megközelítőleg)
	- év: szám
	- hónap.nap: szám.szám (a legtöbbnél csak évet tudunk)
- **képen szereplő emberek**: szöveg (egyelőre!, aztán később ezekből is adatbázis lesz, de mivel nincs névadatbázisunk, ezt majd utólag javítjuk) -> képeslapnál: felismerhető aláírások
	- név1
	- név2
	- …
	- névN 
- **hiányzó adat**: igen/nem (ha a képen nem mindenkit sikerült felismerni, akkor a hiány: igen)
- **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz)
	- címke1
	- címke2
	- …
	- címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)

## Típus: tárgy
- **ID**: ::formátumát még ki kell találni::
- **tárgy megnevezése**: szöveg
- **tárgy képe**: bináris (fotók a tárgyról)
	- kép1/kép2/…/képN
- **tárgy helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a tárgyat) -> ezeket az ID-kat mi határozzuk majd meg!
- **tárgy pozíciója**:
	- dobozban: igen/nem
	- kölcsönben: szöveg (hova adtuk kölcsön)
	- vitrinben: vitrinID
- **tárgy forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsöntárgy**: igen/nem
	- **ha igen**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **tárgy darabszáma**: szám (mennyi van belőle)
- **tárgy típusa**: kupa, trófea / érem / kerámia, porcelán / kitűző / emlék- vagy ajándéktárgy (öngyújtó, kanál, cigarettatárca, etc) / zászló / ruhanemű (cipő, mez, nadrág, sál, etc) / sportszer (labda, síp, karszalag, etc) / ::bővíteni::  / egyéb
- **tárgy keletkezésének ideje**: 
	- bizonytalan: igen/nem
		- **ha** igen: szöveg (megközelítőleg)
	- év: szám
	- hónap.nap: szám.szám (a legtöbbnél csak évet tudunk)
- **tárgyhoz köthető emberek**: szöveg (pl. Tichy Lajos)
- **hiányzó adat**: igen/nem (ha a tárgyról nem tudunk valami komolyabbat, akkor a hiány: igen)
-  **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz)
	- címke1
	- címke2
	- …
	- címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)

## Típus: dokumentum
- **ID**: ::formátumát még ki kell találni::
- **dokumentum megnevezése**: szöveg
- **dokumentum**: bináris (maga a dokumentum)
	- dok1/dok2/…/dokN
- **karakterfelismerés**: igen/nem (ha a dokumentumon elvégeztük a karakterfelismerést, akkor igen) -> **fontos**: a dokumentumnál lehetőséget kell biztosítani a cserére (törlésre), valamint a dokumentumok listázására (előfordulhat, hogy több verzióban fog felkerülni)
- **dokumentum helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a tárgyat) -> ezeket az ID-kat mi határozzuk majd meg!
- **dokumentum pozíciója**:
	- dobozban: igen/nem
	- kölcsönben: szöveg (hova adtuk kölcsön)
	- vitrinben: vitrinID
- **dokumentum forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsöndokumentum**: igen/nem
	- **ha igen**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **dokumentum darabszáma**: szám (mennyi van belőle)
- **dokumentum típusa**:  jegy / bérlet / pass / műsorfüzet / jegyzőkönyv / újság / könyv / brosúra / plakát /  ::bővíteni::  / egyéb
- **dokumentum keletkezésének ideje**: 
	- bizonytalan: igen/nem
		- **ha** igen: szöveg (megközelítőleg)
	- év: szám
	- hónap.nap: szám.szám (a legtöbbnél csak évet tudunk)
- **dokumentumhoz köthető emberek**: szöveg (pl. Tichy Lajos)
	- név1
	- név2
	- …
	- névN 
- **hiányzó adat**: igen/nem (ha a dokumentumról nem tudunk valami komolyabbat, akkor a hiány: igen)
-  **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz)
	- címke1
	- címke2
	- …
	- címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)