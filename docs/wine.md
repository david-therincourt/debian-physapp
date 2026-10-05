# Wine

**Wine** permet d'exécuter des logiciels Windows sous Linux (LTspice…).
On installe ici la version des dépôts Debian, en 64 et 32 bits.

## 1. Activer la section `contrib`

Les paquets `winetricks` et `ttf-mscorefonts-installer` se trouvent dans la section `contrib`
des dépôts Debian, qui n'est pas toujours activée.

Selon l'installation, les dépôts sont déclarés dans `/etc/apt/sources.list.d/debian.sources`
ou dans `/etc/apt/sources.list`. Ouvrez le fichier concerné :

```bash
sudo nano /etc/apt/sources.list.d/debian.sources
```

et complétez chaque ligne `Components:` :

```
Components: main contrib non-free non-free-firmware
```

!!! tip "Remarque"

    Avec l'ancien format `/etc/apt/sources.list`, ajoutez `contrib` à la fin de chaque ligne `deb`, par exemple :
    `deb http://deb.debian.org/debian trixie main contrib non-free non-free-firmware`

## 2. Installer Wine 64 et 32 bits

De nombreux logiciels Windows sont encore en 32 bits : il faut ajouter l'architecture `i386`.

```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install wine wine64 wine32:i386 libwine libwine:i386 fonts-wine
```

## 3. Installer Winetricks et les polices Microsoft

**Winetricks** installe facilement des composants Windows (polices, bibliothèques…)
dans Wine.

```bash
sudo apt install winetricks ttf-mscorefonts-installer
```

!!! tip "Remarque"

    L'installation de `ttf-mscorefonts-installer` demande d'accepter la licence de Microsoft :
    utilisez la touche <kbd>Tab</kbd> pour sélectionner **Ok**, puis **Oui**.

Vérifiez l'installation :

```bash
wine --version
```

## Préfixe 32 bits (anciens logiciels)

Un **préfixe** est un dossier qui contient un « Windows » complet (registre, `C:\`…).
Le préfixe par défaut (`~/.wine`) est en 64 bits. Pour les anciens logiciels
qui ne fonctionnent qu'en 32 bits, créez un préfixe dédié :

```bash
WINEARCH=win32 WINEPREFIX=~/.wine32 winecfg
```

Pour installer ou lancer un logiciel dans ce préfixe, faites précéder la commande
de `WINEPREFIX=~/.wine32`, par exemple :

```bash
WINEPREFIX=~/.wine32 wine setup.exe
```

## Polices pour LTspice

LTspice a besoin des polices Microsoft (Tahoma notamment) pour un affichage correct :

```bash
winetricks corefonts tahoma
```

ou, dans le préfixe 32 bits :

```bash
WINEPREFIX=~/.wine32 winetricks corefonts tahoma
```
