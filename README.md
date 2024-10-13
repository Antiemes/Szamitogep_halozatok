# Alhálózat felosztás

[Segédlet](https://www.youtube.com/watch?v=ikSQoPjdVw4)

# Linux

## A munkakörnyezet

A gyakorlaton Linux alatt fogunk dolgozni. Háromféle rendszert fogunk használni.

  * Fizikai számítógépen futó rendszert
  * Virtuális gépet
  * Távolról elérhető számítógépet

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

### `scp`

```bash
scp forrás cél
```

Akár a forrás, akár a cél lehet helyi, vagy távoli fájl.

### `rsync`

```bash
rsync --partial --progress --inplace --recursive
```

### Hash képzés

Fontosan hash-ek: MD5, SHA1, SHA128, SHA256, SHA512. Az MD5 már nem tekinthető kriptográfiailag
kellően megbízhatónak, de ennek ellenére még használatban van. (Átviteli hibák detektálására alkalmas.)

A következő példák a `file1`, a `file1` és a `file2`, illetve az aktuális könyvtárban található
fájlok MD5, SHA1, SHA128, SHA256 és SHA512 hash-ét számítják ki.

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

### `netcat`

A szerveren *hallgatunk* (listen) egy porton:

```bash
netcat -l -p 12345
```

A kliensen *csatlakozunk* a portra:

```bash
netcat
