# Système

## AppImage (libfuse)

Les applications au format AppImage ont besoin de la bibliothèque FUSE 2.

```bash
sudo apt install libfuse2t64
```

Pour lancer une application AppImage, rendez-la d'abord exécutable :

```bash
chmod +x Application.AppImage
./Application.AppImage
```

!!! tip "Astuce"

    Pour ajouter les AppImage au menu des applications, utilisez **Gear Lever**.
    Il s'installe depuis Flathub (voir [Flatpak et Flathub](#flatpak-et-flathub)) :
    `flatpak install flathub it.mijorus.gearlever`

## Flatpak et Flathub

Installez Flatpak et son module pour la *Logithèque* GNOME, puis ajoutez le dépôt Flathub :

```bash
sudo apt install flatpak gnome-software-plugin-flatpak
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

!!! danger "Important"

    Redémarrez l'ordinateur pour que les applications Flatpak s'affichent dans le menu.

```bash
sudo reboot
```

Facultatif : installez **Flatseal** pour gérer les permissions des applications Flatpak.

```bash
flatpak install flathub com.github.tchx84.Flatseal
```

## Imprimantes : désactiver l'ajout automatique

Par défaut, le service `cups-browsed` détecte les imprimantes partagées sur le réseau
et les ajoute automatiquement. Sur un réseau d'établissement, la liste des imprimantes
se remplit alors d'imprimantes inutiles. Pour désactiver ce comportement :

### 1. Éditer le fichier de configuration

```bash
sudo nano /etc/cups/cups-browsed.conf
```

### 2. Modifier les réglages

Placez ces deux lignes dans le fichier (ou décommentez-les et modifiez-les) :

```
BrowseRemoteProtocols none
CreateIPPPrinterQueues No
```

### 3. Redémarrer le service

```bash
sudo systemctl restart cups-browsed
```

### 4. Supprimer les imprimantes déjà ajoutées (si besoin)

Listez les imprimantes, puis supprimez celles qui ont été créées automatiquement :

```bash
lpstat -v
sudo lpadmin -x NOM_IMPRIMANTE
```

!!! warning "Attention"

    Une mise à jour du paquet `cups-browsed` peut proposer de remplacer ce fichier de configuration.
    Dans ce cas, répondez **N** pour garder votre version.
