# Microcontrôleurs

## Arduino IDE 2

Arduino IDE 2 est distribué pour Linux au format **AppImage**. On le range dans un dossier
`~/Applications`, puis on crée un lanceur pour qu'il apparaisse dans le menu des applications.

!!! danger "Important"

    Prérequis : la bibliothèque FUSE 2 doit être installée
    (voir [AppImage](systeme.md#appimage-libfuse)).

### 1. Télécharger l'AppImage

Téléchargez le fichier `arduino-ide_<version>_Linux_64bit.AppImage` sur la
[page de téléchargement d'Arduino](https://www.arduino.cc/en/software)
ou sur la [page des versions GitHub](https://github.com/arduino/arduino-ide/releases/latest).

### 2. Ranger l'AppImage et la rendre exécutable

Le fichier est renommé `arduino-ide.AppImage` : ainsi, le lanceur reste valable
lors des mises à jour.

```bash
mkdir -p ~/Applications
mv ~/Téléchargements/arduino-ide_*_Linux_64bit.AppImage ~/Applications/arduino-ide.AppImage
chmod +x ~/Applications/arduino-ide.AppImage
```

### 3. Télécharger l'icône

```bash
mkdir -p ~/.local/share/icons
wget -O ~/.local/share/icons/arduino-ide.png \
     https://raw.githubusercontent.com/arduino/arduino-ide/main/electron-app/resources/icons/512x512.png
```

### 4. Créer le lanceur (fichier `.desktop`)

La commande suivante crée le fichier `~/.local/share/applications/arduino-ide.desktop`.
Les chemins sont complétés automatiquement avec le dossier personnel de l'utilisateur :

```bash
mkdir -p ~/.local/share/applications
cat > ~/.local/share/applications/arduino-ide.desktop <<EOF
[Desktop Entry]
Type=Application
Name=Arduino IDE 2
GenericName=Arduino IDE
Comment=Programmation des cartes Arduino
Exec=$HOME/Applications/arduino-ide.AppImage %F
Icon=$HOME/.local/share/icons/arduino-ide.png
Terminal=false
Categories=Development;Electronics;IDE;
MimeType=text/x-arduino;
Keywords=arduino;embedded;electronics;microcontroller;
StartupWMClass=Arduino IDE
EOF
```

Arduino IDE 2 apparaît alors dans le menu des applications. Sinon, fermez puis rouvrez la session.

!!! tip "Remarque"

    Contrairement à Ubuntu, Debian ne bloque pas le bac à sable des applications Electron :
    aucun profil AppArmor n'est nécessaire pour lancer Arduino IDE 2.

### 5. Accès aux cartes

Pour téléverser un programme, l'utilisateur doit appartenir au groupe `dialout`
(voir [Accès aux ports série](vscode.md#ports-serie)).

### Mise à jour

Téléchargez la nouvelle AppImage et refaites l'étape 2 : elle remplace l'ancienne version
sous le même nom, et le lanceur continue de fonctionner.

## ESP32

### Accès à la carte

Les cartes ESP32 apparaissent comme un port série (`/dev/ttyUSB0` ou `/dev/ttyACM0`).
Il suffit que l'utilisateur appartienne au groupe `dialout`
(voir [Accès aux ports série](vscode.md#ports-serie)) :
aucune règle `udev` supplémentaire n'est nécessaire.

!!! tip "Astuce"

    Pour vérifier que la carte est détectée, branchez-la puis lancez `ls /dev/ttyUSB* /dev/ttyACM*`.

### esptool

**esptool** programme les puces ESP32 en ligne de commande : lecture des informations,
effacement de la mémoire flash, écriture d'un firmware (MicroPython par exemple).

```bash
sudo apt install esptool
```

Exemples :

```bash
esptool --port /dev/ttyUSB0 chip_id                 # identifier la puce
esptool --port /dev/ttyUSB0 erase_flash             # effacer la mémoire flash
esptool --port /dev/ttyUSB0 write_flash 0x0 firmware.bin
```

!!! tip "Remarque"

    La version des dépôts Debian (4.7) suffit pour la plupart des cartes. Pour une version plus récente,
    installez esptool avec `pip install esptool` dans un environnement virtuel
    (voir [pip et environnements virtuels](python.md#pip-et-environnements-virtuels)) :
    n'utilisez pas l'option `--break-system-packages`.

### Avec Arduino IDE 2

Dans Arduino IDE 2 : **Outils → Carte → Gestionnaire de cartes**, recherchez **esp32**
et installez le paquet **esp32 by Espressif Systems**.

### Avec MicroPython

Après avoir écrit le firmware MicroPython avec esptool, la carte se programme
avec [Thonny](python.md#thonny).

## STM32

### Outils ST-LINK et règles d'accès

Les cartes STM32 (Nucleo, Discovery…) se programment par la sonde **ST-LINK** intégrée.
Le paquet `stlink-tools` fournit les outils en ligne de commande **et installe les règles `udev`**
d'accès aux sondes ST-LINK : il n'y a rien à créer à la main.

```bash
sudo apt install stlink-tools
```

Débranchez puis rebranchez la carte, et vérifiez qu'elle est détectée :

```bash
st-info --probe
```

Écriture d'un programme compilé (`.bin`) dans la mémoire flash :

```bash
st-flash write programme.bin 0x8000000
```

!!! tip "Astuce"

    Les cartes Nucleo apparaissent aussi comme une clé USB (`NOD_xxx`) : il suffit d'y copier
    le fichier `.bin` pour programmer la carte. Leur port série virtuel (`/dev/ttyACM0`)
    nécessite l'appartenance au groupe `dialout`.

### OpenOCD (débogage)

**OpenOCD** permet de déboguer pas à pas un programme sur la carte (avec GDB, VS Code…).
Il installe lui aussi ses règles `udev`.

```bash
sudo apt install openocd
```
