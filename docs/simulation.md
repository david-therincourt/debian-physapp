# Simulation

## LTspice (avec Wine)

**LTspice** est un simulateur de circuits électroniques (SPICE) gratuit, édité par Analog Devices.
Il n'existe que pour Windows et macOS : sous Linux, on l'utilise avec Wine.

!!! danger "Important"

    Wine doit être installé au préalable, avec les polices Microsoft
    (voir la page [Wine](wine.md)).

### 1. Télécharger l'installateur

Téléchargez l'installateur 64 bits `LTspice64.msi` depuis le site d'Analog Devices :

```bash
cd ~/Téléchargements
wget https://ltspice.analog.com/software/LTspice64.msi
```

### 2. Installer LTspice

```bash
wine msiexec /i LTspice64.msi
```

Suivez l'assistant d'installation en laissant les options par défaut.

### 3. Installer les polices

Sans les polices Microsoft, les menus et les schémas de LTspice s'affichent mal :

```bash
winetricks corefonts tahoma
```

### 4. Lancer LTspice

LTspice apparaît dans le menu des applications. On peut aussi le lancer depuis un terminal :

```bash
wine ~/.wine/drive_c/users/$USER/AppData/Local/Programs/ADI/LTspice/LTspice.exe
```

!!! tip "Astuce"

    Les fichiers de circuits (`.asc`) peuvent être enregistrés dans le dossier personnel Linux :
    il est accessible depuis LTspice par le lecteur `Z:` (`Z:\home\<utilisateur>\…`).
