# PCB

## KiCad

**KiCad** est une suite libre de conception électronique : saisie de schémas,
routage de circuits imprimés (PCB), visualisation 3D et génération des fichiers de fabrication.

Debian 13 fournit KiCad 9.0 dans ses dépôts, mais dans une version figée.
Deux méthodes permettent d'avoir une version plus récente :

| Méthode               | Version (octobre 2026) | Mises à jour                  |
|:----------------------|:-----------------------|:------------------------------|
| Rétroportages Debian  | 9.0.x (corrections)    | avec le système (`apt`)       |
| Flatpak (Flathub)     | 10.0.x                 | avec `flatpak update`         |

!!! warning "Attention"

    Un projet enregistré avec KiCad 10 ne peut plus être ouvert avec KiCad 9.
    Utilisez la même version sur tous les postes de la salle et à la maison.

### Option 1 : rétroportages Debian (*backports*)

Les **rétroportages** (*trixie-backports*) proposent des versions plus récentes de certains
logiciels, recompilées pour Debian 13.

Activez le dépôt des rétroportages :

```bash
sudo tee /etc/apt/sources.list.d/debian-backports.sources > /dev/null <<'EOF'
Types: deb
URIs: http://deb.debian.org/debian
Suites: trixie-backports
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
EOF
sudo apt update
```

Installez KiCad, ses bibliothèques et sa documentation en français depuis les rétroportages :

```bash
sudo apt install -t trixie-backports kicad kicad-libraries kicad-doc-fr
```

!!! tip "Remarque"

    L'option `-t trixie-backports` est nécessaire : sans elle, `apt` installe la version
    des dépôts principaux. Les mises à jour suivantes des rétroportages sont ensuite
    appliquées normalement par `sudo apt upgrade`.

!!! tip "Astuce"

    Le paquet `kicad-packages3d` (modèles 3D des composants, installé avec `kicad-libraries`)
    pèse plusieurs gigaoctets. Pour l'éviter, installez seulement
    `kicad kicad-symbols kicad-footprints kicad-doc-fr` : la visualisation 3D affichera
    alors les cartes sans les composants.

### Option 2 : Flatpak

KiCad s'installe depuis Flathub
(voir [Flatpak et Flathub](systeme.md#flatpak-et-flathub)) :

```bash
flatpak install flathub org.kicad.KiCad
```

Les bibliothèques de symboles et d'empreintes sont installées automatiquement avec KiCad.
Si les modèles 3D des composants n'apparaissent pas dans la visualisation 3D, installez-les :

```bash
flatpak install flathub org.kicad.KiCad.Library.Packages3D
```

## FlatCAM 2024.4 (AppImage)

**FlatCAM** prépare la fabrication des circuits imprimés par gravure mécanique (fraiseuse CNC) :
à partir des fichiers Gerber et de perçage exportés par KiCad, il calcule les trajets
d'isolation, de perçage et de découpe, puis génère le G-code.

!!! danger "Important"

    Prérequis : la bibliothèque FUSE 2 doit être installée
    (voir [AppImage](systeme.md#appimage-libfuse)).

### 1. Télécharger et ranger l'AppImage

Téléchargez le fichier `flatcam-2024.4-x86_64.AppImage`, puis rangez-le dans `~/Applications`
et rendez-le exécutable :

```bash
mkdir -p ~/Applications
mv ~/Téléchargements/flatcam-2024.4-x86_64.AppImage ~/Applications/
chmod +x ~/Applications/flatcam-2024.4-x86_64.AppImage
```

### 2. Correctif pour la langue française (indispensable)

Avec une session en français (`fr_FR.UTF-8`), FlatCAM 2024.4 plante au démarrage avec l'erreur :

```
TypeError: Couldn't build proto file into descriptor pool:
Invalid default '0.5' for field operations_research.sat.SatParameters.clause_cleanup_ratio of type 1
```

**Cause :** la virgule décimale française empêche la bibliothèque `ortools` (protobuf)
de lire les valeurs comme `0.5`.

**Solution :** lancer FlatCAM avec la convention numérique C (point décimal) :

```bash
LC_NUMERIC=C ~/Applications/flatcam-2024.4-x86_64.AppImage
```

!!! tip "Remarque"

    `LC_NUMERIC=C` ne modifie que l'écriture des nombres : l'interface reste en français.
    Ce réglage garantit aussi un G-code avec des points décimaux ; un G-code avec des virgules
    serait refusé par les commandes de fraiseuse (GRBL, Wegstr…).

### 3. Créer le lanceur (fichier `.desktop`)

La commande suivante crée le lanceur, avec le correctif `LC_NUMERIC=C` intégré.
Les chemins sont complétés automatiquement avec le dossier personnel de l'utilisateur :

```bash
mkdir -p ~/.local/share/applications
cat > ~/.local/share/applications/flatcam.desktop <<EOF
[Desktop Entry]
Type=Application
Name=FlatCAM 2024.4
Comment=FAO pour circuits imprimés (isolation, perçage, découpe)
Exec=env LC_NUMERIC=C $HOME/Applications/flatcam-2024.4-x86_64.AppImage
Terminal=false
Categories=Development;Engineering;Electronics;
EOF
```

!!! tip "Astuce"

    Pour lancer FlatCAM depuis un terminal avec le correctif, ajoutez un alias à la fin de `~/.bashrc` :
    `alias flatcam='LC_NUMERIC=C ~/Applications/flatcam-2024.4-x86_64.AppImage'`,
    puis rechargez le fichier avec `source ~/.bashrc`.

### 4. Préférences au premier lancement

- **Édition → Préférences → Général → Paramètres GUI** : décochez **« Utiliser des icônes grises »**.
- **Édition → Préférences → Général → Langue** : sélectionnez **Français**,
  appliquez, puis redémarrez FlatCAM.

### 5. Vérification

Au lancement, FlatCAM doit :

1. démarrer sans erreur `TypeError` (protobuf/ortools) ;
2. afficher l'interface en français avec des icônes en couleur ;
3. exporter un G-code avec des points décimaux (à vérifier dans un fichier `.nc` généré).
