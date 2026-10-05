# Bureautique

## LibreOffice en français

Installez LibreOffice avec l'interface et l'aide en français, l'intégration à GNOME
et les outils linguistiques :

```bash
sudo apt install libreoffice libreoffice-l10n-fr libreoffice-help-fr \
                 libreoffice-gnome hunspell-fr hyphen-fr mythes-fr
```

| Paquet                | Rôle                                   |
|:----------------------|:---------------------------------------|
| `libreoffice-l10n-fr` | Interface en français                  |
| `libreoffice-help-fr` | Aide en français                       |
| `libreoffice-gnome`   | Intégration à GNOME (boîtes de dialogue, thème) |
| `hunspell-fr`         | Correcteur orthographique              |
| `hyphen-fr`           | Césure (coupure des mots)              |
| `mythes-fr`           | Dictionnaire des synonymes             |

### Polices compatibles Microsoft Office

Les polices **Carlito** et **Caladea** ont les mêmes dimensions que Calibri et Cambria.
Elles évitent les décalages de mise en page à l'ouverture des fichiers `.docx`, `.xlsx` et `.pptx`.

```bash
sudo apt install fonts-crosextra-carlito fonts-crosextra-caladea
```

### Correcteur grammatical (facultatif)

L'extension **Grammalecte** ajoute un correcteur grammatical et typographique pour le français.
Téléchargez le fichier `.oxt` sur [grammalecte.net](https://grammalecte.net), puis dans LibreOffice :
**Outils → Gestionnaire des extensions → Ajouter**.

## PDF Arranger

**PDF Arranger** permet de fusionner, découper, réordonner, faire pivoter
et supprimer les pages de fichiers PDF, par simple glisser-déposer.

```bash
sudo apt install pdfarranger
```

## Capture d'écran : Ksnip

**Ksnip** réalise des captures d'écran (plein écran, fenêtre, zone rectangulaire)
et permet de les annoter : flèches, cadres, texte, numéros, flou…

```bash
sudo apt install ksnip
```

!!! tip "Remarque"

    Sous Wayland, Ksnip passe par le portail de capture de GNOME : une fenêtre de
    confirmation s'affiche à chaque capture. La capture d'une seule fenêtre peut
    ne pas être disponible.

## Retouche de captures : Gradia

**Gradia** met en valeur les captures d'écran : ajout d'un fond (couleur, dégradé, image),
de marges, de coins arrondis et d'annotations, puis export ou copie dans le presse-papiers.

Gradia s'installe depuis Flathub (voir [Flatpak et Flathub](systeme.md#flatpak-et-flathub)) :

```bash
flatpak install flathub be.alexandervanhee.gradia
```
