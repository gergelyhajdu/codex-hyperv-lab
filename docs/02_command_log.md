# Codex Hyper-V Lab – Parancsnapló

Ez a dokumentum időrendben rögzíti a Codex Hyper-V Lab kialakítása és az első kontrollált teszt során ténylegesen használt és ellenőrzött parancsokat.

A grafikus felületen végzett fontosabb beállítások részletesebb magyarázata és a kapcsolódó képernyőképek az [`01_environment_setup.md`](01_environment_setup.md) dokumentumban találhatók.

## 01. Tiszta Windows 11 alapállapot ellenőrzése

A Windows 11 telepítése és frissítése után elkészült az első Hyper-V-ellenőrzőpont:

```text
00-win11-clean-updated
```

A tiszta ellenőrzőpont visszaállítása után az alábbi paranccsal ellenőrizhető, hogy a Codex CLI még nincs telepítve:

```powershell
where.exe codex
```

Az elvárt eredmény:

```text
INFO: Could not find files for the given pattern(s).
```

## 02. Elkülönített standard felhasználó ellenőrzése

A Codex CLI használatához létrejött egy külön helyi, standard jogosultságú Windows-felhasználó:

```text
CodexLabUser
```

A külön felhasználóval történt bejelentkezés után az aktuális Windows-identitás ellenőrzése:

```powershell
whoami
```

A kapott eredmény:

```text
gyak-codex-ai\codexlabuser
```

## 03. Standard felhasználói alapállapot mentése

A külön standard felhasználó létrehozása és ellenőrzése után Hyper-V-ellenőrzőpont készült:

```text
01-standard-user-ready
```

## 04. Codex CLI telepítése a standard felhasználói profilba

A Codex CLI Windowsos telepítője normál, nem emelt jogosultságú PowerShell-munkamenetből futott:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

A telepítés után egy új PowerShell-ablakban ismét ellenőrzésre került az aktuális felhasználó és a telepített Codex CLI verziója:

```powershell
whoami
codex --version
```

A kapott eredmény:

```text
gyak-codex-ai\codexlabuser
codex-cli 0.139.0
```

## 05. Codex CLI telepítése utáni alapállapot mentése

Az első Codex-indítás és hitelesítés előtt új Hyper-V-ellenőrzőpont készült:

```text
02-standard-user-codex-cli-installed
```

## 06. Elkülönített munkamappa létrehozása

```powershell
New-Item -ItemType Directory -Path "$HOME\CodexLab\01-first-test" -Force
Set-Location "$HOME\CodexLab\01-first-test"
Get-Location
Get-ChildItem -Force
```

Az ellenőrzött munkakönyvtár:

```text
C:\Users\CodexLabUser\CodexLab\01-first-test
```

A könyvtár az első Codex-indítás előtt üres volt.

## 07. ChatGPT-fiókos hitelesítés

```powershell
whoami
codex login
```

A böngészőben megjelenő URL-ek, egyszer használatos kódok és egyéb hitelesítési adatok nem kerültek dokumentálásra.

A sikeres bejelentkezés után az aktív hitelesítési mód ellenőrzése:

```powershell
codex login status
```

A kapott eredmény:

```text
Logged in using ChatGPT
```

## 08. Az első interaktív Codex-munkamenet ellenőrzése

A Codex CLI az elkülönített munkamappából indult el:

```powershell
Set-Location "$HOME\CodexLab\01-first-test"
Get-Location
codex
```

A munkamenet aktuális állapotának ellenőrzése a Codex felületén:

```text
/status
```

Az ellenőrzés alapján:

```text
Modell: gpt-5.5
Munkakönyvtár: ~\CodexLab\01-first-test
Jogosultsági profil: Workspace (Ask for approval)
Projekt-specifikus AGENTS.md fájl: nincs
Hitelesítés: ChatGPT-fiókkal
```

## 09. Első módosítás nélküli Codex-teszt

Az első interaktív teszt célja annak ellenőrzése volt, hogy a Codex képes-e kizárólag olvasási műveletet végezni, és betartja-e a fájlmódosítás tiltását.

A Codex az alábbi feladatot kapta:

```text
Ne módosíts semmilyen fájlt, és ne futtass módosítást végző parancsot.
Ellenőrizd, hogy az aktuális munkakönyvtár üres-e, majd röviden írd le,
milyen lépésekkel hoznál létre benne egy hello.txt fájlt.
A fájlt egyelőre ne hozd létre.
```

A Codex kizárólag az alábbi olvasási parancsot futtatta:

```powershell
Get-ChildItem -Force
```

A parancs nem adott vissza fájlt vagy mappát, ezért az aktuális munkakönyvtár üres volt.

![Első módosítás nélküli Codex-teszt](../images/03_first_controlled_test/17-first-read-only-codex-test.png)

## 10. Első kontrollált fájllétrehozás

A következő teszt célja egyetlen, pontosan meghatározott tartalmú fájl kontrollált létrehozása volt.

A Codex az alábbi feladatot kapta:

```text
Hozz létre az aktuális munkakönyvtárban egy hello.txt nevű fájlt pontosan ezzel a tartalommal:

Hello from the Codex Hyper-V Lab.

Ezután olvasd vissza a fájlt, és ellenőrizd, hogy a tartalma pontosan megfelelő-e.

Ne hozz létre és ne módosíts más fájlt.
Ne lépj ki az aktuális munkakönyvtárból.
Ne használj hálózati hozzáférést.
```

A Codex a fájl létrehozása előtt megmutatta a tervezett módosítást, és manuális jóváhagyást kért.

![A fájllétrehozás jóváhagyása](../images/03_first_controlled_test/18-first-controlled-file-creation-approval.png)

A létrehozás után a Codex visszaolvasta a fájl tartalmát. Észlelte, hogy az első megoldás a fájl végére lezáró sortörést helyezett, ezért külön jóváhagyást kért a pontos tartalom visszaírásához.

![A pontos tartalom visszaírásának jóváhagyása](../images/03_first_controlled_test/19-exact-content-rewrite-approval.png)

A fájl a Windows grafikus felületén is megnyitható volt.

![A létrehozott hello.txt fájl](../images/03_first_controlled_test/20-hello-txt-created-and-opened.png)

Ezt követően a Codex külön ellenőrző paranccsal hasonlította össze a fájl tartalmát az elvárt szöveggel.

Az ellenőrzés eredménye:

```text
EXACT_MATCH
```

![A pontos tartalom ellenőrzésének eredménye](../images/03_first_controlled_test/21-exact-content-verification-result.png)

## 11. Független PowerShell-ellenőrzés

A Codex saját visszajelzése után külön, a Codex-munkameneten kívül is ellenőrzésre került a munkakönyvtár tartalma és a létrehozott fájl pontos szövege.

A Codex interaktív munkamenetéből az alábbi slash paranccsal történt kilépés:

```text
/quit
```

A fájlrendszer és a fájltartalom ellenőrzése normál PowerShell-ablakban történt:

```powershell
Set-Location "$HOME\CodexLab\01-first-test"

Get-ChildItem -Force

Get-Content -Raw -LiteralPath .\hello.txt

$actual = [System.IO.File]::ReadAllText(
    (Join-Path (Get-Location) 'hello.txt'),
    [System.Text.UTF8Encoding]::new($false)
)

$actual -ceq 'Hello from the Codex Hyper-V Lab.'
```

Az ellenőrzés alapján:

```text
A munkakönyvtárban található egyetlen fájl: hello.txt
A fájl tartalma: Hello from the Codex Hyper-V Lab.
A pontos szövegegyezés eredménye: True
```

![Független PowerShell-ellenőrzés](../images/03_first_controlled_test/22-independent-powershell-verification.png)

## 12. Korábbi Codex-munkamenet folytatása

A legutóbbi, aktuális munkakönyvtárhoz tartozó interaktív munkamenet az alábbi paranccsal lett ismét megnyitva:

```powershell
codex resume --last
```

A visszatöltött felületen megjelent a korábbi beszélgetés és az előző feladat eredménye:

```text
EXACT_MATCH
```

![A korábbi Codex-munkamenet folytatása](../images/03_first_controlled_test/23-resume-last-session.png)

Az első kontrollált teszt sikeresen lezárult.
