# Alhálózat felosztás

[Segédlet](https://www.youtube.com/watch?v=ikSQoPjdVw4)

# Linux

## A munkakörnyezet

A gyakorlaton Linux alatt fogunk dolgozni. Kétféle rendszert fogunk használni.

  * A laborgépen futó rendszert
    - Manjaro Linux KDE környezettel
    - Felhasználónevünk: `hallgato`
  * Távolról elérhető számítógépet
    - Debian Linux
    - A laborhálózatról a 10.22.3.216 IP címen érhető el
    - Felhasználónevünk: Neptun kód csupa kisbetűvel
    - Alapértelmezett jelszó: 123, amit az első órák egyikén megváltoztattunk

## Hálózati munkához kapcsolódó / általános Linux parancsok, programok

### `w`

A `w` parancs listázza a bejelentkezett felhasználókat, illetve a rendszer állapotáról
(uptime, terheltség) ad tájékoztatást. Jelzi többek közt a felhasználók loginnevét,
illetve hogy mit futtatnak éppen.

### `last`

A `last` parancs a felhasználók belépési / kilépési naplóját mutatja meg, illetve
hogy melyik terminálon, vagy távoli belépés esetén milyen IP címről lépett be az illető.

### `ip`

Az `ip` egy sokoldaló konfigurációs eszköz, ami többek közt a hálózati interfészek
állapotáról tud információt adni. Interfészek címeinek lekérdezése:

```bash
ip address show
```

A parancsot rövidíthetjük, például `ip addr show` formában. Az `address`-hez tartozó
alapértelmezett akció a `show`, így azt elhagyhatjuk és használhatjuk az `ip address`,
`ip addr`, vagy egyszerűen `ip a` verziót is.

### `ssh`

Az SSH (Secure Shell) egy távoli gépre történő biztonságos parancssori (shell) belépésre
használható, amennyiben ez a távoli gépen engedélyezve van és rendelkezünk a megfelelő
belépési adatokkal. Használata:

```bash
ssh user@host
```

A `user` a felhasználónevünket jelenti. Ez a mező (a `@` jellel együtt) elhagyható,
ekkor a parancs a jelenlegi felhasználónevöóünket használja.

A `host` mező domain nevet és IP címet is jelenthet.

### `ping`

A `ping` paranccsal a hálózati kapcsolatot tudjuk tesztelni például a következő
formában:

```bash
ping host
```

Ez a parancs másodpercenként fog egy üzenetváltást indítani és folyamatosan működik, addig,
amíg a programot le nem állítjuk. A parancs számos paraméterét szabályozhatjuk,
például az elküldendő üzenetek számát, gyakoriságát, méretét stb.

Fontos megjegyezni, hogy a `ping` az ICMP protokollt, illetve annak echo és echo reply
üzeneteit használja, így előfordulhat, hogy ezek tiltása miatt a célállomást nem érjük el,
de egyéb protokollokkal igen, illetve ugyanez fordítva is lehetséges.

### `scp`

Az SCP (Secure Copy) segítségével egy távoli gép és a sajátunk között tudunk biztonságosan
fájlokat másolni. Általában ahova az SSH belépés engedélyezett, ott az SCP-t is tudjuk
használni, de lehetnek kivételek mindkét irányban.

Az SCP használata:

```bash
scp forrás cél
```

Akár a forrás, akár a cél lehet helyi, vagy távoli fájl. A helyi fájlokat azok útvonalával
adjuk meg. Ez lehet abszolút, vagy relatív útvonalmegadás is.

A távoli gépen levő fájl megadásához először egy, az SSH-nál is használt `user@host`
kombináció kell. Ezt követi egy `:` (**kettőspont**), majd végül az útvonal.

Ha a cél a távoli gépen van, akkor az útvonal elhagyható (de a `:`-ot ebben az esetben is
ki kell írni!). Ilyenkor a fájl a forrással azonos néven a távoli gépen levő home
könyvtárunkba kerül.

Néhány példa:

(A példák mindegyikében a távoli gépet az `1.2.3.4`-es IP címmel azonosítjuk, felhasználónevünk
pedig `user`.)

A helyi gépen levő `valami.txt` nevű fájlt a távoli gépen a saját home könyvtárunkba másoljuk.

```bash
scp valami.txt user@1.2.3.4:
```

A helyi gépen levő `valami.txt` nevű fájlt a távoli gépen a `/tmp` könyvtárba másoljuk.

```bash
scp valami.txt user@1.2.3.4:/tmp
```

A helyi gépen levő `valami.txt` nevű fájlt a távoli gépen a `/tmp` könyvtárba másoljuk `masik.txt` néven.

```bash
scp valami.txt user@1.2.3.4:/tmp/masik.txt
```

A távoli gépen a saját home könyvtárunkban levő `valami.txt` nevű fájlt lemásoljuk a helyi
gépen az aktuális könyvtárba. (Az aktuális könyvtárt `.` (pont) jelöli.)

```bash
scp user@1.2.3.4:valami.txt .
```

A távoli gépen a `/tmp` könyvtárban levő `valami.txt` nevű fájlt lemásoljuk a helyi
gépen az aktuális könyvtárba.

```bash
scp user@1.2.3.4:/tmp/valami.txt .
```

A távoli gépen a `/var/www` könyvtárban levő `valami.txt` nevű fájlt lemásoljuk
a helyi gép `/tmp` könyvtárába.

```bash
scp user@1.2.3.4:/var/www/valami.txt /tmp
```

A távoli gépen a `/var/www` könyvtárban levő `valami.txt` nevű fájlt lemásoljuk
a helyi gép `/tmp` könyvtárába `uj.txt` néven.

```bash
scp user@1.2.3.4:/var/www/valami.txt /tmp/uj.txt
```

### `rsync`

```bash
rsync --partial --progress --inplace --recursive
```

### TCP kapcsolat létrehozása `netcat`-tel

A szerveren *hallgatunk* (listen) egy porton:

```bash
netcat -l -p 12345
```

Szükségünk van a szerver IP címére is, amit az `ip a` paranccsal tudunk lekérdezni. Legyen most a szerver
IP címe `1.2.3.4`.

A kliensen *csatlakozunk* a portra:

```bash
netcat 1.2.3.4 12345
```

### `wget` és `curl`

## Kódolási eljárások és kriptográfia

### BASE64 kódolás

Kódolás:

```bash
base64 szoveg.txt > kodolt.txt
```

Dekódolás:

```bash
base64 -d kodolt.txt
```

### Hash képzés

Fontosabb hash-ek: MD5, SHA1, SHA128, SHA256, SHA512. Az MD5 már nem tekinthető kriptográfiailag
kellően megbízhatónak, de ennek ellenére még használatban van. (Átviteli hibák detektálására alkalmas.)

A következő példák a `file1`, a `file1` és a `file2`, illetve az aktuális könyvtárban található
össes fájl MD5, SHA1, SHA128, SHA256 és SHA512 hash-ét számítják ki.

```bash
md5sum file1
md5sum file1 file2
md5sum *

sha1sum file1
sha1sum file1 file2
sha1sum *

sha128sum file1
sha128sum file1 file2
sha128sum *

sha256sum file1
sha256sum file1 file2
sha256sum *

sha512sum file1
sha512sum file1 file2
sha512sum *
```

### Nyilvános kulcs alapú autentikáció

A nyilvános kulcsú authentikációval jelszó helyett kulcs segítségével léphetünk be egy távoli
számítógépre. Ehhez a helyi gépen el kell készíteni egy (titkos és nyilvános kulcsból álló)
*kulcspárt*, amiből a *nyilvános kulcsot* fel kell másolni a távoli gépre.

A kulcspár előállítása (a helyi gépen):

```bash
ssh-keygen
```

A nyilvános kulcs a `.ssh/id_rsa.pub` néven, a titkos kulcs `.ssh/id_rsa` néven keletkezik.

### Szimmetrikus kulcsú titkosító használata

Számos kommunikációs protokoll tartalmaz beépített titkosítási lehetőséget, illetve számos egyéb szoftver is
alkalmas ilyen megoldást. A továbbiakban az OpenSSL példáján keresztül fogjuk megvizsgálni a titkosító
eljárásokat.

Az OpenSSL-en keresztül mind szimmetrikus, mind nyílt kulcsú titkosítást tudunk hasznáni. Most csak az előbbivel
fogunk foglalkozni. A gyakorlatban használt szimmetrikus titkosítási eljárások szinte kivétel nélkül blokk kódolók.
Ahhoz, hogy ezek valamelyikét használni tudjuk, a következőkre van szükségünk:

 * A titkosító eljárás kiválasztására (ez lehet például **DES, 3DES, vagy AES**).
 * A blokkméret és a kulcsméret kiválasztása (amennyiben többfélével is működik az algoritmus). Az AES esetében **128, 192 és 256 bites** verziók közül választhatunk.
 * A blokk kódoló használati módjának kiválasztására. Ez **ECB, CBC, OFB, CFB, vagy CTR** lehet. Az OpenSSL nem implementálja az összes lehetséges kombinációt.
 * A kulcselőállátási algoritmusra (key derivation function). Ez a továbbiakban minden esetben a **PBKDF2** lesz. Ez az algoritmus egy jelszóból egy, a titkosításhoz szükséges kulcsot tud előállítani. A további példákban azt is megadjuk, hogy a jelszó legyen **sózva**.

#### Titkosítás

A következő példában a `nyiltszoveg.txt` fájlt titkosítjuk el, ezzel előállítva a `titkosszoveg.enc` fájlt. Az `aes-256-cbc` paraméter
AES titkosítást, 256 bites működést és *CBC* módot állít be. A fentiek alapján egyéb kombinációk is lehetségesek.

```bash
openssl aes-256-cbc -a -salt -pbkdf2 -in nyiltszoveg.txt -out titkosszoveg.enc
```

#### Visszafejtés

A visszafejtésnél a `titkosszoveg.enc` fájlt fejtjük vissza, ezzel előállítva az `uj_nyiltszoveg.txt`-t. A visszafejtésnél
ugyanazt a működési módot kell használni, mint a titkosításnál.

```bash
openssl aes-256-cbc -d -a -pbkdf2 -in titkosszoveg.enc -out uj_nyiltszoveg.txt
```

A lehetséges kulcsméretek tehát az AES-hez:

  * aes-128
  * aes-192
  * aes-256


A lehetséges blokk módok:

  * ecb
  * cbc
  * ofb
  * cfb
  * ctr

#### Az ECB mód sérülékenysége

Próbáljuk ki az ecb (elektronikus kódkönyv) módot is. Vigyázat! Ez a mód nem biztonságos.

(TBD)

### Digitális aláírás

A titkosító algoritmusokhoz hasonlóan digitális aláírást is számos szoftver implementál. Most a GPG (GNU Privacy Guard)
példáján keresztül vizsgáljuk ezt meg. A GPG segítségével egy dokumentumot (tetszőleges fájlt) láthatunk el digitális aláírással,
melyet természetesen ellenőrizni is tudunk. A digitális aláírás nem jelent titkosítást!

#### Kulcsok listázása

A digitális aláírás nyílt kulcsú titkosító algoritmust használ (amivel azonban nem magát a dokumentumot titkosítja!).
Ahhoz, hogy a egy dokumentumot aláírhassunk, szükség van a kulcspár titkos részére (a titkos kulcsra). Az
aláírás ellenőrzéséhez ugyanezen pár nyilvános részére (a nyilvános kulcsra) van szükség.

A kulcsainkat a következő módon listázhatjuk:

Nyilvános kulcsok:

```bash
gpg --list-keys
```

Titkos kulcsok:

```bash
gpg --list-secret-keys
```

#### Kulcspár generálása

Egy kulcspárt (ami egy titkos és egy nyilvános kulcsból áll) a következő paranccsal állíthatunk elő.

```bash
gpg --gen-key
```

A titkos kulcsot egy jelszóval is levédhetjük. Ilyenkor a GPG minden alkalommal, amikor a titkos kulcshoz hozzá szeretnénk férni,
ezt a jelszót fogja kérni tőlünk.

#### Publikus kulcs exportálása

Ahhoz, hogy valaki ellenőrizni tudja az aláírt dokumentumot, a mi nyilvános kulcsunkra van szüksége. Ezt valamilyen módon ki kell nyernünk és
mint fájlt kell továbbítanunk a másik személynek.

A kulcs kinyerése (`kulcs.gpg`) fájlba a következő paranccsal történik. Az egyik parancs a megadott e-mail cím alapján azonosítja a kulcsot,
a másik pedig a hash alapján. (A kulcslistában mindkettőt látjuk.)

```bash
gpg --output kulcs.gpg --export valaki@pelda.com
```

Vagy:

```bash
gpg --output kulcs.gpg --export HASH
```

A `--armor` kapcsolóval ASCII formátumú kulcsot kapunk, anélkül binárisat. Bármelyiket használhatjuk.

A `kulcs.gpg` minden esetben a *publikus* kulcsunkat tartalmazza. Ahhoz, hogy valaki ellenőrizni tudja
azt, amit én aláírok, rendelkeznie kell ezzel a publikus kulccsal.

 * Aláírás: *titkos kulcssal*
 * Ellenőrzés: *publikus kulcssal*

#### Aláírás

A következőkben az `uzenet.txt` fájlt fogjuk aláírni. (Tehát valamilyen tanult módon, például `mcedit` segítségével
először el kell készíteni az `uzenet.txt` fájlt, aminek a tartalma tetszőleges lehet.)

Az aláírást kétféle módon tudjuk elvégezni. Az első módszer használatával egy olyan kimeneti fájl keletkezik, amelyik *egyben* tartalmazza
az eredeti dokumentumot és a digitális aláírást. Ennek előnye, hogy mivel egyetlen fájl hordozza a dokumentumot és az aláírást, ezek
nem tudnak egymástól függetlenül "elkeveredni". Hátránya, hogy így az eredeti fájlhoz közvetlenül nem férünk hozzá.
(Ez egyben előny is lehet, hiszen így egy véletlen automatikus mentéssel nem fogjuk a dokumentumot akaratlanul módosítani.)

Az aláírás létrehozása ezesetben a következő:

```bash
gpg --sign uzenet.txt
```

Ez a parancs az `uzenet.txt` fájlt írja alá. A keletkezett új file neve mindig az eredeti, plusz a `.gpg` toldalék. Formátuma bináris.
Generálhatunk ASCII formátumú aláírást is a `--armor` kapcsolóval. A `--default-key` kapcsoló után egy kulcsot választhatunk név, vagy
hash alapján. Például:

```bash
gpg --sign --armor --default-key 'vg@electrit.hu' uzenet.txt
```

A `--armor` kapcsolóval a keletkezett fájl ASCII formátumú lesz.


A másik módszer szerint egy *leválasztott* aláírást hozunk létre. Ekkor az eredeti fájl változatlan formában megmarad és használható.
Ez főleg nagy méretű, nem módosítandó fájlok (például CD és DVD képfájlok) esetében hasznos.

Itt az alap parancs a következő:


```bash
gpg --detach-sign uzenet.txt
```

Az egyes kiegészítő kapcsolók (`--armor`, `--default-key`) itt is használhatóak.

#### Ellenőrzés

Az ellenőrzéshez először importálni kell a publikus kulcsot. A következő példában a kulcs a `kulcs.gpg` fájlban van:

```bash
gpg --import kulcs.gpg
```

Ezután az ellenőrzés a következő módon tehető meg:


```bash
gpg --verify uzenet.txt.asc
```

#### Kulcsok törlése

```bash
gpg --delete-secret-key
```

```bash
gpg --delete-key
```

