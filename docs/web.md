# Web

## Chromium

Installez Chromium et sa traduction française :

```bash
sudo apt install chromium chromium-l10n
```

!!! tip "Remarque"

    Préférez le paquet Debian au Flatpak : il donne accès aux API **Web Serial** et **WebUSB**,
    utiles pour programmer des cartes (ESP32, Arduino, micro:bit…) depuis le navigateur.

### Affichage natif sous Wayland

Pour que Chromium utilise Wayland directement (affichage plus net, meilleure prise en charge
des écrans HiDPI) :

```bash
echo 'export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --ozone-platform-hint=auto"' | sudo tee /etc/chromium.d/wayland
```

Relancez Chromium pour appliquer le réglage.

### Accélération vidéo (facultatif)

Installez le pilote VA-API correspondant à la carte graphique :

| Carte graphique | Paquet                   |
|:----------------|:-------------------------|
| Intel           | `intel-media-va-driver`  |
| AMD             | `mesa-va-drivers`        |

```bash
sudo apt install intel-media-va-driver   # Intel
sudo apt install mesa-va-drivers         # AMD
```
