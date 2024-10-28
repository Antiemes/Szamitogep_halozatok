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


