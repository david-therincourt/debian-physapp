# Interfaces

## Analog Devices ADALM2000 (M2K)

### Pilote et bibliothèque libm2k

Les dépôts Debian fournissent la bibliothèque **libm2k**, l'outil en ligne de commande `m2kcli`
et la bibliothèque Python :

```bash
sudo apt install libiio-utils m2kcli python3-libm2k
```

!!! tip "Remarque"

    Les règles d'accès USB à la carte sont installées automatiquement avec la bibliothèque :
    il n'y a rien à configurer.

Branchée en USB, la carte apparaît comme une interface réseau à l'adresse `192.168.2.1` :

```bash
iio_info -u ip:192.168.2.1
```

### Scopy

**Scopy** est le logiciel d'instrumentation d'Analog Devices : oscilloscope, générateur de signaux,
analyseur de spectre, analyseur logique, alimentations…

Téléchargez le fichier `Scopy-v<version>-Linux-x86_64.AppImage` sur la
[page des versions de Scopy](https://github.com/analogdevicesinc/scopy/releases/latest),
puis rendez-le exécutable (voir [AppImage](systeme.md#appimage-libfuse)) :

```bash
cd ~/Téléchargements
chmod +x Scopy-*-Linux-x86_64.AppImage
./Scopy-*-Linux-x86_64.AppImage
```

!!! tip "Remarque"

    La même page propose aussi Scopy au format Flatpak (`Scopy-v<version>-Linux-x86_64.flatpak`),
    à installer avec `flatpak install --user Scopy-*.flatpak`. Cette installation est plus lourde,
    car elle télécharge en plus un environnement d'exécution complet.

### Python

La bibliothèque `libm2k` installée plus haut s'utilise directement en Python :

```python
import libm2k

ctx = libm2k.m2kOpen("ip:192.168.2.1")
ctx.calibrateADC()
ctx.calibrateDAC()
print(ctx.getSerialNumber())
libm2k.contextClose(ctx)
```

## Digilent Analog Discovery 3

### Pilotes et logiciel WaveForms

Téléchargez sur le [site de Digilent](https://digilent.com/reference/software/waveforms/waveforms-3/start)
les paquets `.deb` pour Linux (amd64) :

- **Adept 2 Runtime** (`digilent.adept.runtime_<version>-amd64.deb`) ;
- **Adept 2 Utilities** (`digilent.adept.utilities_<version>-amd64.deb`) ;
- **WaveForms** (`digilent.waveforms_<version>_amd64.deb`).

Installez-les ensemble avec `apt`, qui récupère aussi les dépendances manquantes
(bibliothèques Qt…) :

```bash
cd ~/Téléchargements
sudo apt install ./digilent.adept.runtime_*.deb ./digilent.adept.utilities_*.deb \
                 ./digilent.waveforms_*.deb
```

!!! tip "Astuce"

    Avec `dpkg -i`, les dépendances ne sont pas installées et il faut ensuite lancer
    `sudo apt --fix-broken install`. `apt install ./fichier.deb` fait tout en une seule fois.

### WaveForms

**WaveForms** regroupe les instruments de la carte : oscilloscope, générateur de signaux,
analyseur logique, analyseur de réseau, alimentations…

Lancement depuis le menu des applications ou avec la commande `waveforms`.
