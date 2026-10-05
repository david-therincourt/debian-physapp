# Radio logicielle (SDR)

## GNU Radio

**GNU Radio** permet de construire des chaînes de traitement du signal sous forme de
schémas-blocs, avec l'éditeur graphique **GNU Radio Companion**.

```bash
sudo apt install gnuradio
```

Lancement : `gnuradio-companion`.

## Gqrx SDR

**Gqrx** est un récepteur radio logiciel : affichage du spectre et de la cascade (*waterfall*),
démodulation AM, FM, BLU…

```bash
sudo apt install gqrx-sdr
```

## Carte USRP B200 (Ettus Research)

### Pilote

```bash
sudo apt install uhd-host
```

!!! tip "Remarque"

    Sur Debian 13, le paquet `uhd-host` installe lui-même les règles d'accès aux cartes USRP
    (`/usr/lib/udev/rules.d/60-uhd-host.rules`) : il n'y a rien à copier à la main.

### Téléchargement des images FPGA

La carte charge son firmware et son image FPGA à chaque branchement.
Téléchargez ces images une fois pour toutes :

```bash
sudo uhd_images_downloader
```

### Détection de la carte

```bash
uhd_find_devices
```

### Utilisation

| Logiciel  | Prise en charge          |
|:----------|:-------------------------|
| GNU Radio | Bloc **USRP Source**     |
| Gqrx      | Prise en charge native   |

## Carte ADALM-Pluto (Analog Devices)

### Pilote

```bash
sudo apt install libiio-utils
```

### Informations sur la carte

Branchée en USB, la carte apparaît comme une interface réseau à l'adresse `192.168.2.1` :

```bash
iio_info -u ip:192.168.2.1
```

### Session SSH

```bash
ssh root@192.168.2.1
```

Le mot de passe par défaut est `analog`.

### Utilisation

| Logiciel  | Prise en charge             |
|:----------|:----------------------------|
| GNU Radio | Bloc **PlutoSDR Source**    |
| Gqrx      | Non pris en charge          |

### Python (pyadi-iio)

La bibliothèque **pyadi-iio** permet de piloter la carte depuis Python. Elle n'existe pas
dans les dépôts Debian : on l'installe avec `pip` dans un environnement virtuel
(voir [pip et environnements virtuels](python.md#pip-et-environnements-virtuels)).

```bash
sudo apt install python3-libiio
python3 -m venv --system-site-packages ~/venv
source ~/venv/bin/activate
pip install pyadi-iio
```

!!! tip "Astuce"

    L'option `--system-site-packages` permet à l'environnement d'utiliser la bibliothèque
    `libiio` installée avec `apt`.
