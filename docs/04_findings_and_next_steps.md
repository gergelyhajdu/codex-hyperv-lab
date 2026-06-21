# Tapasztalatok és következő lépések

## Eddigi tapasztalatok

### A Codex CLI telepítése felhasználói profilhoz kötődik

A labor kialakításakor a CLI végül kizárólag a külön létrehozott `CodexLabUser` standard jogosultságú profiljába került telepítésre.

Ez egyszerűbbé és egyértelműbbé teszi a jogosultsági határok dokumentálását.

### A PowerShell ISE nem megfelelő az interaktív Codex-felülethez

A Codex interaktív terminálfelülete PowerShell ISE-ből nem indítható el megfelelően, mert az ISE kimeneti ablaka nem valódi terminál.

Az interaktív CLI használatához normál Windows PowerShell vagy Windows Terminal szükséges.

### Az emelt Windows-sandbox beállítása standard felhasználói környezetben nem futott le

A Codex első indításakor megjelent az emelt jogosultságot igénylő natív Windows-sandbox beállítási lehetősége.

A labor célja a standard jogosultságú felhasználói környezetben végzett kontrollált tesztelés volt. Ebben a munkamenetben az emelt sandbox automatikus beállítása nem futott le sikeresen, mert a Windows az alábbi segédprogramot nem találta:

```text
codex-windows-sandbox-setup.exe
```

A kezdeti tesztek ezért a nem rendszergazdai Windows-sandbox használatával futottak. Ez illeszkedett a labor védelmi modelljéhez, de az emelt sandbox működésének részletesebb vizsgálata későbbi feladat marad.

### A manuális jóváhagyás működött

A `Workspace (Ask for approval)` jogosultsági profil mellett a Codex a módosítások és egyes parancsok futtatása előtt külön jóváhagyást kért.

Ez lehetőséget adott arra, hogy a tervezett műveletek végrehajtás előtt ellenőrizhetők legyenek.

### A pontos követelmény apró eltérést is felszínre hozott

A `hello.txt` első változata lezáró sortörést tartalmazott. A Codex ezt visszaolvasáskor észlelte, majd külön jóváhagyás után korrigálta.

Ez jól szemlélteti, hogy a pontos specifikáció és az eredmény visszaellenőrzése kis feladatnál is fontos.

### A Codextől független ellenőrzés továbbra is szükséges

A Codex saját visszajelzése után külön PowerShell-ellenőrzés is történt. Ez igazolta, hogy csak az engedélyezett `hello.txt` jött létre, és annak tartalma pontosan megfelelő.

## Következő tervezett lépések

A labor után a következő nagyobb gyakorlati irány a `multi-rdbms-data-extraction-lab` projekt lett. Ez jó folytatása ennek a munkának, mert az AI-támogatott fejlesztést már nem önmagában, hanem egy adatkinyerési és több adatbázist érintő portfólióprojekt mellett lehet tovább vizsgálni.

A további lépések:

- a Codex használatának kipróbálása mesterséges, kontrollált adatfeldolgozási feladatokon;
- egyszerű PowerShell- vagy Python-script létrehozása mesterséges bemenettel;
- hibás script elemzése és javítása;
- fájlmódosítások áttekintése `/diff` használatával;
- hosszabb specifikáció kipróbálása;
- több fájlból álló mesterséges projekt vizsgálata;
- később adatfeldolgozási és PL/SQL-jellegű gyakorlófeladatok;
- minden eredmény manuális ellenőrzése, mielőtt portfólió-dokumentációba kerül.
