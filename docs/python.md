# Python

Python 3 est installé par défaut sur Debian 13. Tous les outils ci-dessous
s'installent depuis les dépôts Debian avec `apt`.

## pip et environnements virtuels

```bash
sudo apt install python3-pip python3-venv python-is-python3
```

!!! tip "Remarque"

    Debian ne fournit que la commande `python3`. Le paquet `python-is-python3` ajoute
    la commande `python`, que les étudiants tapent souvent par habitude (Windows, tutoriels).

!!! danger "Important"

    Sur Debian 13, `pip install` est **refusé en dehors d'un environnement virtuel**
    (erreur `externally-managed-environment`) afin de ne pas casser les paquets Python du système.

Deux façons d'installer une bibliothèque :

1. **Depuis les dépôts Debian** (à privilégier quand le paquet existe) :

   ```bash
   sudo apt install python3-numpy python3-matplotlib python3-scipy python3-pandas
   ```

2. **Avec pip, dans un environnement virtuel** :

   ```bash
   python3 -m venv ~/venv          # créer l'environnement (une seule fois)
   source ~/venv/bin/activate      # l'activer
   pip install nom_du_paquet       # installer
   deactivate                      # quitter l'environnement
   ```

!!! tip "Remarque"

    Un **environnement virtuel** est un dossier qui contient sa propre copie de Python et ses propres
    bibliothèques, isolées de celles du système. Les paquets installés avec `pip` dans cet environnement
    ne modifient pas le Python de Debian et n'entrent pas en conflit avec les paquets installés par `apt`.

    On peut créer un environnement par projet, chacun avec ses propres versions de bibliothèques.
    Pour repartir de zéro, il suffit de supprimer le dossier de l'environnement.

    Une fois l'environnement **activé**, son nom s'affiche au début de l'invite du terminal, par exemple
    `(venv) david@poste:~$`. Les commandes `python` et `pip` utilisent alors cet environnement
    jusqu'à ce qu'on le quitte avec `deactivate`.

!!! tip "Astuce"

    Avec l'option `--system-site-packages` (`python3 -m venv --system-site-packages ~/venv`),
    l'environnement voit aussi les bibliothèques installées avec `apt` (NumPy, Matplotlib…) :
    pas besoin de les réinstaller avec `pip`.

## Thonny

**Thonny** est un éditeur Python simple, adapté à l'apprentissage :
débogueur pas à pas, affichage des variables, gestion des paquets.
Il permet aussi de programmer les cartes **MicroPython** (ESP32, Raspberry Pi Pico…).

```bash
sudo apt install thonny
```

!!! tip "Remarque"

    Pour programmer une carte branchée en USB, l'utilisateur doit appartenir au groupe `dialout`
    (voir [Accès aux ports série](vscode.md#ports-serie)).

## Spyder

**Spyder** est un environnement de développement scientifique, proche de MATLAB :
éditeur, console IPython, explorateur de variables et affichage des graphiques.

```bash
sudo apt install spyder
```

## Jupyter Notebook

**Jupyter Notebook** permet de créer des carnets qui mêlent code, résultats,
graphiques et texte mis en forme, dans le navigateur.

```bash
sudo apt install jupyter-notebook
```

Lancement depuis le dossier de travail :

```bash
jupyter notebook
```

## JupyterLab

**JupyterLab** est l'interface plus moderne de Jupyter : onglets, explorateur de fichiers,
terminal et éditeur de texte intégrés. Il ouvre les mêmes carnets (`.ipynb`) que Jupyter Notebook.

```bash
sudo apt install jupyterlab
```

Lancement depuis le dossier de travail :

```bash
jupyter lab
```

!!! tip "Astuce"

    Jupyter s'ouvre dans le navigateur. Pour l'arrêter, revenez dans le terminal
    et appuyez sur <kbd>Ctrl</kbd>+<kbd>C</kbd>.
