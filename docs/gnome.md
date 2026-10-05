# Gnome

## Ajustements et extensions

Installez l'outil **Ajustements** (*GNOME Tweaks*) et les outils de gestion des extensions :

```bash
sudo apt install gnome-tweaks gnome-shell-extensions \
                 gnome-shell-extension-manager gnome-shell-extension-prefs \
                 gnome-browser-connector
```

- **Ajustements** : polices, boutons des fenêtres, applications au démarrage…
- **Gestionnaire d'extensions** (*Extension Manager*) : rechercher, installer et configurer des extensions.
- **gnome-browser-connector** : installer des extensions depuis le site [extensions.gnome.org](https://extensions.gnome.org).

## Extension Dash to Dock

Debian livre un GNOME « vanilla », sans dock permanent. L'extension **Dash to Dock** en ajoute un.

```bash
sudo apt install gnome-shell-extension-dashtodock
gnome-extensions enable dash-to-dock@micxgx.gmail.com
```

!!! danger "Important"

    Sous Wayland, fermez puis rouvrez la session pour que l'extension soit prise en compte.

!!! tip "Remarque"

    Si la version packagée n'est pas compatible avec GNOME 48, installez Dash to Dock
    depuis le **Gestionnaire d'extensions**. Autre possibilité : l'extension **Dash to Panel**.

## Extension Apps Menu

L'extension **Apps Menu** ajoute un menu « Applications » classique, rangé par catégories,
dans la barre supérieure. Elle fait partie du paquet `gnome-shell-extensions` installé plus haut :
il suffit de l'activer.

```bash
gnome-extensions enable apps-menu@gnome-shell-extensions.gcampax.github.com
```

!!! tip "Astuce"

    Vous pouvez aussi activer ou désactiver les extensions depuis l'application **Extensions**
    ou le **Gestionnaire d'extensions**.

## Éditeur de menu : Libre Menu Editor

**Libre Menu Editor** (*Main Menu*) permet de modifier les entrées du menu des applications :
renommer, changer l'icône ou la commande, masquer une application, créer un lanceur
(par exemple pour une AppImage ou un logiciel lancé avec Wine).

Il n'est pas dans les dépôts Debian : il s'installe depuis Flathub
(voir [Flatpak et Flathub](systeme.md#flatpak-et-flathub)) :

```bash
flatpak install flathub page.codeberg.libre_menu_editor.LibreMenuEditor
```

## Nautilus : aperçu rapide avec Sushi

**GNOME Sushi** affiche un aperçu du fichier sélectionné quand on appuie sur la touche
<kbd>Espace</kbd> dans Nautilus. Il est installé par défaut. Sinon :

```bash
sudo apt install gnome-sushi
```

Pour avoir aussi l'aperçu des fichiers audio et vidéo, installez les codecs GStreamer :

```bash
sudo apt install gstreamer1.0-libav gstreamer1.0-plugins-good gstreamer1.0-plugins-bad
```
