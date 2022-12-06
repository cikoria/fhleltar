# Futball Ház leltárprogram specifikáció

## Tartalomjegyzék

- [Általános leírás](#általános-leírás)
  - [Alapfiunkciók](#alapfunkciók)
  - [Szószedet](#szószedet)
- [Admin felület](#admin-felület)
  - [Felhasználók, jogkezelés](#felhaszn%C3%A1l%C3%B3i-kateg%C3%B3ri%C3%A1k-%C3%A9s-jogosults%C3%A1gok)
    - [Felhasználói kategóriák és jogosultságok](#felhasználói-kategóriák-és-jogosultságok)
  - [Rekord létrehozása/szerkesztése](#rekord-létrehozásaszerkesztése)
    - [Szerkesztő blokk](#szerkesztő-blokk)
    - [Bináris állományok kezelése](#bin%C3%A1ris-%C3%A1llom%C3%A1nyok-kezel%C3%A9se)
    - [Sematikus ábra](#sematikus-ábra)
  - [Rekordok státuszai, listázása, adatlapja](#rekordok-státuszai-listázása-adatlapja)
    - [Rekordok státuszai](#rekordok-státuszai)
    - [Rekordok láthatósága](#rekord-láthatósága)
    - [Rekordok listázása](#rekordok-listázása)
    - [Funkciók a rekordok listában](#funkciók-a-rekordok-listájában)
    - [Rekordok adatlapja](#rekordok-adatlapja-f%C3%A1zis-2)
- [Adattípusok](#adattípusok)
  - [Típus: kép](#típus-kép)
  - [Típus: tárgy](#típus-tárgy)
  - [Típus: dokumentum](#típus-dokumentum)

- - - -

## Általános leírás

A Futball Ház leltárprogram egy egyedi igényekre szabott adatbázis-keretrendszer. A leltárprogram elsődleges funkciója a Kispesti Futball Ház állományában lévő tárgyak nyilvántartása. A leltárprogram másodlagos funkciója ezen tárgyak publikálásának, szélesebb nyilvánosság felé történő bemutatásának elősegítése.

**[!!!]** A specifikáció **[opcionális]** címkével jelöli az **alacsony prioritású** fejlesztési feladatokat. Ezek nélkül a rendszer működőképes, legfeljebb pár funkció eléréséhez fejlesztőre lesz szükség. Néhol az **[opcionális]** helyett a tervezett fázis szerepel címkeként, tehát például **[Fázis 2.]**

### Alapfunkciók

Alapfunkciónak nevezzük azokat a funkciókat, amelyekre mindenképp szükség van ahhoz, hogy a rendszert működőnek tekinthessük.

- Fázis 1. (alapfunkciók)
  - felhasználók autentikációja (be- és kijelentkezés)
  - egyetlen felhasználótípus van: adminisztrátor
  - új rekord létrehozása (minden attribútomot rögzíteni kell)
  - bináris állományok feltöltésének lehetősége a rekordokhoz
  - rekord mentése
  - rekord szerkesztése
  - rekord törlése
  - rekordok listázása
- Fázis 2.
  - új felhasználótípus: látogató
  - adatlap létrehozása
  - rekordok láthatóságának figyelembe vétele
  - rekordok listázásánál a felhasználótípus figyelembe vétele
- Fázis 3.
  - új felhasználótípus: feltöltő
  - rekord véglegesíte kapcsoló figyelembe vétele
- Fázis 4.
- Fázis 5.
- Fázis N+1. (nincs specifikálva)
  - rekorodok egy csoportja kijelölhető arra, hogy autentikáció nélkül is megtekinthető legyen az adatlapja
  - saját oldal a szabadon hozzáférhető rekordok böngészéséhez

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
- **[opcionális]** felhasználókezelés
- **[opcionális]** törölt rekordok kezelése

Az admin felület sematikus felépítése:

- fejléc: kijelentkezés
- bal oldalon: menürendszer
  - **[opcionális]** profil
  - rekordok: a leltárban lévő **véglegesített** rekordok listája
  - véglegesítés: **csak** az erre jogosult felhasználóknál jelenik meg (vagy nekik aktív)
  - **[opcionális]** törölt: **csak** az erre jogosult felhasználóknál jelenik meg (vagy nekik aktív)
- jobb oldalt: menürendszerben navigálástól függő tartalom
  - **[opcionális]** profil: jelszóváltoztatási lehetőség
  - rekordok: a felhasználó jogosultságának megfelelő rekordok listázása, illetve, ha a jogosultsága engedi, akkor az oldalon atkív `új rekord` létrehozása gomb
  - véglegesítés: a felhasználó jogosultságának megfelelő rekordok listázása, valamint az oldalon atkív `új rekord` létrehozása gomb. Az erre jogosult felhasználók itt véglegesíthetik a feltöltött rekordokat, amelyek ezt követően kerülnek át a zárt/kutatható/publikus kategóriákba.
  - **[opcionális]** törölt: a felhasználó jogosultságának megfelelő rekordok listázása. Az erre jogosult felhasználók itt állíthatják vissza a törölt rekordokat.

A felhasználók számára kilistázott rekordok **két helyen** kattinthatóak:

- a rekordba bárhova kattinva: a rekord adatlapjára navigál.
- a rekord végén lévő `szerkesztés` gombra kattintva a rekord létrehozó/szerkesztő oldalára navigál.

```text
FHleltár       |                                    logout  ... |
---------------|------------------------------------------- ... |
Profil >       | | ID    | típus      | név                 ... |
Rerkordok >    | |-------|------------|-------------------- ... |
Véglegesítés > | | fh235 | fotó       | Csapatkép 1929-ből  ... |
Törölt >       | | fh236 | dokumentum | Fegyelmi határozat  ... |
```

### Felhasználók, jogkezelés

**[opcionális]** A felhasználók felhasználónévvel és jelszóval jelentkeznek be. A rendszer a felhasználót alapértelmezett jelszóval hozza létre, majd a felhasználók azt az admin felületen megváltoztatják. Kerüljük el az `Almafa123` típusú jelszavakat.

#### Felhasználói kategóriák és jogosultságok

**[opcionális]** Az admin felületen az adminisztrátor jogkörű felhsználók létrehozhatnak adott jogkörökkel új felhasználókat. Ennek a felületnek a megléte **nem prioritás**, bőven elégséges, ha adatbázisban létrehozhatók a felhasználók.

- adminisztrátor
  - bejelentkezés
  - **[opcionális]** felhasználók: felhasználó létrehozása, listázása, jogosultságának megváltoztatása
  - rekord létrehozása, feltöltés megkezdése, mentése
  - rekord végelegesítése,
  - véglegesített rekord státuszának megváltoztatása nem véglegesre
  - rekord törlése
  - **publikus**, **kutatható** és **zárt** rekordok listázása
- **[Fázis 3.]** feltöltő
  - bejelentkezés
  - rekord létrehozása, feltöltés megkezdése, mentése
  - nem véglegesített rekordok szerkesztése
  - **publikus**, **kutatható** és **zárt** rekordok listázása
- **[opcionális]** kutató
  - bejelentkezés
  - **publikus** és **kutatható** rekordok listázása
- **[Fázis 2.]** látogató
  - bejelentkezés
  - **publikus** rekordok listázása

### Rekord létrehozása/szerkesztése

Rekordot az erre jogosult felhasználók a rekordokat listázó oldalak belső fejlécében, az `új rekord` gombra kattintva hozhatak létre.

A rekordok létrehozása és szerkesztése ugyanazon az oldalon történik. Attól függően, hogy mi a rekord státusza, a felhasználó a rekkordot létrehozza, vagy szerkeszti.

A rekord létrehozásakor automatikusan létrejön a rekord egyedi azonosítója (ID). **[opcionális]** feladat, hogy ez az azaonosító a későbbiekben szerkeszthető legyen.

A három rekordtípusnak (kép, tárgy, dokumentum) némileg eltérő a létrehozási/szerkesztési felülete. A specifikáció törekszik arra, hogy a lehető legtöbb közös jellemezőjét meghatározza a rekordtípusoknak, egyszerűsítve ezzel a rekrodtípusok kezelését.

A rekordoktípusok létrehozásához tartozó mezők specifikációját lásd a dokumentum [Adattípusok](#adattípusok) fejezetében.

#### Szerkesztő blokk

A közös blokk minden adattípusnál fixen jelen van a rekord felvitele és szerkesztése oldalon.

- rekord láthatóságának beállítása (zárt, kutatható, publikus)
- rekord véglegesítése váltókapcsoló
- mentés
- törlés

**[opcionális]** A rekordok nyomonkövetése szempontjából hasznos lenne, ha néhány adatot tárolnánk a rekordok életciklusából.

- **[opcionális]** a rekordoknál megjelenítésre kerül a rekord története:
  - ki hozta létre a rekordot és mikor
  - ki szerkesztette utoljára a rekordot és mikor
  - **[opcionális]** a szerkesztési előzmények listázása -> user és dátum

#### Bináris állományok kezelése

A rekord létrehozása/szerkesztése oldalon történik a bináris állományok (képek, dokumentumok) kezelése. A felhasználóknak itt lehetősége kell legyen:

- feltölteni egy vagy több bináris állományt,
- megjelölni az egyik bináris állományt, mint elsődleges állomány,
- törölni egy feltöltött bináris állományt. (A törölt állományt felesleges megtartani.)

A későbbiekben az elsődlegesnek megjelölt bináris állomány fog szerepelni a rekord adatlapján kiemelt állományként (praktikusan képként).

#### Sematikus ábra

```text
Rekord létrehozása/szerkesztése
------------------------------------------------

ID: fh235 [szerkesztés]

Megnevezés: ...........................
Rekord helye (doboz): .................
(...)
(...)
Leírás:
┌--------------------------------------┐
| [B] [I]                              |
|--------------------------------------|
| Lorem ipsum sic doloret              |
|                                      |
└--------------------------------------┘
------------------------------------------------
[Állomány feltöltése]
Feltöltött állomány1 [elsődleges] (X) [törlés]
(...)
Feltöltött állományN [elsődleges] ( ) [törlés]
------------------------------------------------
Láthatóság [zárt|kutatható|publikus] 
Véglegesítés [I|N] 
[Mentés] [Törlés]
------------------------------------------------
Rekordot létrehozta: user, dátum
Utolsó módosítás: user, dátum
```

### Rekordok státuszai, listázása, adatlapja

#### Rekordok státuszai

Egy rekord (kép, tárgy, dokumentum) a létrehozás (feltöltés) során több státuszon megy keresztül:

1. létrehozás -> létrejön a rekord ID-ja
2. feltöltés -> ez egy köztes státusz, amikor a rekord adatai feltöltésre kerülnek, azonban **nem** az összes -> a feltöltés a `mentés` gomb megnyomásával zárul
   1. a feltöltést bárki végezheti, azonban
   2. feltöltést, tehát rekordot csak erre jogosult felhasználó véglegesíthet
3. véglegesítés -> az erre jogosult felhasználó fejezi be a feltöltést (például meghatározza a rekord tárgyának fizikai helyét (dobozID), valamint ellenőrzi a felvitt adatokat) -> a feltöltés a `rekord véglegesítése` váltókapcsoló átállításával és a `mentés` gomb megnyomásával zárul (a váltókapcsoló csak az arra jogosult felhasználók számra aktív)
4. törölt -> speciális státusz.
   1. ebben az állapotban **nem** kerül fizikailag törlésre a rekord, hanem kap egy törölt flaget, és átkerül a törölt elemek listába
   2. a `törlés` gombot csak az erre jogosult felhasználók nyomhatják meg (a gomb biztonsági kérdést jelenít meg: biztos/mégsem)

#### Rekord láthatósága

A rekordok láthatóságának figyelembe vétele a **[Fázis 2.]** része, azonban a **[Fázis 1.]** során kell lekódolni a jelölő beállításának lehetőségét, illetve menteni a rekordoknál beállított értékeket.

Az egyes rekordoknak három láthatósági állapota létezik, amikre **kizárólag** az adminisztrátor jogkörrel rendelkező felhasználók állíthatnak be (a feltöltők számára inaktív attribútum). A rekordok láthatósága **csak a véglegesített** rekordokon értelmezhető, tehát a nem véglegesített rekordok kizárólag a feltöltési státusz listájából érhetők el az arra jogosult felhasználóknak.

A rekordokat a felhasználók a leltárprogram admin felületén listázhatják ki, és az egyes rekordokra kattintva a rekord adatlapjára kerülnek. A felhasználói jogosultságok növekményesek, vagyis az egyes szintekhez hozzátartoznak az összes alacsonyabb szint jogosultságai is.

1. zárt -> kizárólag az adminisztrátor és feltöltő jogosultságú felhasználók számára kerül listázásra.
2. **[opcionális]** kutatható -> a legalább kutató jogosultságú felhasználók számára kerül listázásra.
3. publikus rekord -> minden felhasználó számára listázásra kerül.

Alapértelmezés: publikus rekord.

```text
├── véglegesítésre váró rekord
├── véglegesített rekord
    ├── zárt rekord
    ├── kutatható rekord
    ├── publikus rekord
├── törölt
```

#### Rekordok listázása

A leltárprogram adminisztrációs felületén külön listák segítik a feltöltést végző felhasználókat:

1. befejezett feltöltések -> itt a véglegesített rekordokat listázzuk
2. nyitott feltöltések -> a véglegesítésre váró feltöltések
3. törölt feltöltések

#### Funkciók a rekordok listájában

A rekordokat listázó felületeken (a Rekordok és a Véglegesítés menüpontok alatt, valamint értelemszerűen az **[opcionális]** Törölt menüpont alatt) a felhasználók számára a következő lehetőségeket kell biztosítani:

1. `új rekord` hozzáadása -> egyetlen kérdés: a rekord típusa (kép, tárgy, dokumentum)
2. rekordok listázása és egyértelmű beazonosítása legalább a következő elemekkel:
   1. ID
   2. rekord típusa (kép, tárgy, dokumentum)
   3. rekord neve (a megnevezés)
   4. rekord helye (dobozID)
   5. rekord pozíciója (vitrinID)
   6. rekord láthatósága
   7. `szerkesztés` gomb -> a rekord feltöltési/szerkesztési oldalára visz
3. rekord adatlapja -> (a rekordon bárhova kattintva) a rekord adatlapjára navigál
4. táblázatfejlécben rendezési lehetőség
5. az adminisztrációs lapon keresési lehetőség (szabadszavas)
   1. **[opcionális]** a keresőmező mellett egy legördülő listából kiválasztható, hogy a felhasználó pontosan melyik mezőben szeretne keresni (például: címkék)
6. lapozási lehetőség -> ha túl sok a megjeleníthető rekord, akkor a felhasználók lapozhassanak a listázott elemek között
7. **[opcionális]** a felhasználók az oldal alján egy legördülő menüben megadhatják, hogy mennyi rekordot szeretnének listázni (25, 50, 100, 250)

```text
[+ rekord hozzáadása]                      [mező^] keresés: ...............

| ID    | típus      | név                | doboz  | pozíció | láthatóság |
|-------|------------|--------------------|--------|---------|------------|---
| fh235 | kép        | Csapatkép 1929-ből | dob045 | vitN3   | publikus   | E
| fh236 | dokumentum | Fegyelmi határozat | dob012 |         | kutatható  | E
| fh237 | tárgy      | Bozsik-féle váza   | dob004 |         | zárt       | E

elemszám: [20^]              « < - > »
```

#### Rekordok adatlapja [Fázis 2.]

A rekord adatlapja egy egyedi stíluslappal ellátott oldal, ahol az adott rekord adatait meg lehet tekinteni. -> A későbbi publikálás alapját ezek az oldalak képezik.

A rekordok adatlapját a rekordok listájából, az adott rekordra kattintva lehet elérni. Mivel a listák eleve jogkezelést alkalmaznak, így az egyes rekordok adatlapjaihoz csak az arra jogosult felhasználók férhetnek hozzá.

A rekordok adatlapján **csak** azok az adatok kerülnek listázásra, amelyek közvetlenül a rekordhoz köthetők, tehát

- a rekordot adminisztráló felhasználók adatai **nem**
- a rekordhoz tartozó státusz és egyéb információk **nem**
- a rekord fizikai helyéből csak a vitrin azonosítója.
  - **HA** egy rekord tárgya nincs vitrinben, akkor `raktárban` felirat olvasható helyette.
- **[opcionális]** nyomtatási kép, vagy valami egyszerű sablon -> a rekord adatlapját böngésző felhasználó másolatot készíthet magának (PDF exportálás)

- - - -

## Adattípusok

**[opcionális]** Minden rekordokhoz tartozik egy **leírás mező**, amely hosszú szöveget tartalmaz. Mivel ez a mező szolgálna az adott rekord részletes leírására, jó lenne, ha támogatna pár alapvető formázási lehetőséget: paragrafus, félkövér betű, dőlt betű.

### Típus: kép

- **ID**:
- **kép megnevezése**: szöveg
- **kép**: bináris, maga a képállomány
- **hátlap**: bináris, képállomány (ha a hátlapon van valami)
  - **[opcionális]** **kép felbontása**: ha ki tudod olvasni, akkor tároljuk
- **kép színei**: színes / hamisszínes / festett / fekete-fehér / etc
- **fotó állapota**: elmosódott / hiányos / összefirkált / etc
- **kép raktári helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a képet) -> ezeket az ID-kat mi határozzuk majd meg!
- **kép pozíciója**: bevételezés alatt (amikor még nincs pozíciója) / dobozban (vagyis raktáron) / kölcsönben (valahova kiadva) / vitrinben (vagyis a kiállítótérben) / digitális (vagyis csak az adatbázisunkban)
- **kép forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsönkép**: igen/nem
  - **HA IGEN**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **hivatkozás**: URL (ha a kép a neten is elérhető)
- **kép mérete**: kicsi / közepes / nagy (szemre határozzuk meg)
- **kép darabszáma**: szám (mennyi van belőle)
- **kép típusa**: meccs / csapatkép / portré / (...) / egyéb
- **kép helyszíne**:
  - **ismeretlen**: igen/nem
  - **HA NEM**: szöveg (egyelőre!, a kép készítésének helye, majd később lehet, ebből is rendezett lista lesz)
- **kép készítésének ideje**:
  - **ismert dátum**: igen/nem
    - **HA IGEN**: év.hó.nap
    - **HA NEM**: szöveg
- **képen szereplő emberek (képeslapnál: aláírások)**: 
  - **elemszám**: szám (mennyi ember/aláírás van a képen)
  - **nevek**: név1 / név2 / ... / névN
- **dedikált**: igen/nem
  - **HA IGEN**: név1 / név2 / ... / névN
- **hiányzó adat**: igen/nem (ha a képen nem mindenkit sikerült felismerni, akkor a hiány: igen)
- **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz) -> pl. ha keretben van, az is egy címke!
  - címke1 / címke2 / … / címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)

### Típus: tárgy

- **ID**:
- **tárgy megnevezése**: szöveg
- **tárgy képe**: bináris (fotók a tárgyról)
  - kép1 / kép2 / … / képN
- **tárgy állapota**: hiányos / törött / etc
- **tárgy helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a tárgyat) -> ezeket az ID-kat mi határozzuk majd meg!
- **tárgy pozíciója**: bevételezés alatt (amikor még nincs pozíciója) / dobozban (vagyis raktáron) / kölcsönben (valahova kiadva) / vitrinben (vagyis a kiállítótérben) / ~~digitális (vagyis csak az adatbázisunkban)~~
- **tárgy forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsöntárgy**: igen/nem
  - **HA IGEN**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **tárgy darabszáma**: szám (mennyi van belőle)
- **tárgy típusa**: szöveg [kupa, trófea / érem / kerámia, porcelán / kitűző / emlék- vagy ajándéktárgy (öngyújtó, kanál, cigarettatárca, etc) / zászló / ruhanemű (cipő, mez, nadrág, sál, etc) / sportszer (labda, síp, karszalag, etc) / (...)  / egyéb]
- **tárgy keletkezésének ideje**:
  - **ismert dátum**: igen/nem
    - **HA IGEN**: év.hó.nap
    - **HA NEM**: szöveg
- **a tárgyhoz köthető emberek**: szöveg (pl. Tichy Lajos)
  - név1 / név2 / … / névN
- **ellenféltől kapott tárgy**: igen/nem 
  - **HA IGEN**: szöveg
- **hiányzó adat**: igen/nem (ha a tárgyról nem tudunk valami komolyabbat, akkor a hiány: igen)
- **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz)
  - címke1 / címke2 / … / címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)

### Típus: dokumentum

- **ID**:
- **dokumentum megnevezése**: szöveg
- **dokumentum**: bináris (maga a dokumentum)
  - dok1 / dok2 / … / dokN
- **karakterfelismerés**: igen/nem (ha a dokumentumon elvégeztük a karakterfelismerést, akkor igen) -> **fontos**: a dokumentumnál lehetőséget kell biztosítani a cserére (törlésre), valamint a dokumentumok listázására (előfordulhat, hogy több verzióban fog felkerülni)
- **eredeti példány**: igen/nem (HA például a Honvédnál van az eredeti, akkor a Honvéd egy önálló dobozID lesz)
- **dokumentum állapota**: hiányos / sérült / elmosódott / etc
- **dokumentum helye**: dobozID/albumID, szöveg (vagyis ahol fizikailag tároljuk a tárgyat) -> ezeket az ID-kat mi határozzuk majd meg!
- **dokumentum pozíciója**: bevételezés alatt (amikor még nincs pozíciója) / dobozban (vagyis raktáron) / kölcsönben (valahova kiadva) / vitrinben (vagyis a kiállítótérben) / digitális (vagyis csak az adatbázisunkban)
- **dokumentum forrása**: szöveg (kitől kaptuk, mikor, esetleg jogtulajdonos, etc)
- **kölcsöndokumentum**: igen/nem
  - **HA IGEN**: szöveg (megjegyzés, hogy kitől kaptuk, satöbbi)
- **dokumentum darabszáma**: szám (mennyi van belőle)
- **dokumentum típusa**:  jegy / bérlet / pass / műsorfüzet / jegyzőkönyv / újság / könyv / brosúra / plakát /  szerződés (játékos, szponzor, más egyesülettel, etc) / (...)  / egyéb
- **dokumentum keletkezésének ideje**:
  - **ismert dátum**: igen/nem
    - **HA IGEN**: év.hó.nap
    - **HA NEM**: szöveg
- **dokumentumhoz köthető emberek**: szöveg (pl. Tichy Lajos)
  - név1 / név2 / … / névN
- **dedikált**: igen/nem
  - **HA IGEN**: név1 / név2 / ... / névN
- **hiányzó adat**: igen/nem (ha a dokumentumról nem tudunk valami komolyabbat, akkor a hiány: igen)
- **címkék**: szöveg (egyelőre! aztán lehet, ebből is rendezett lista lesz)
  - címke1 / címke2 / … / címkeN
- **leírás**: hosszú szöveg (szabadszavas mező)
