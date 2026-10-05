# VS Code

## Installation depuis le dépôt APT de Microsoft

Le dépôt officiel de Microsoft permet d'installer VS Code avec `apt`
et de recevoir les mises à jour avec le reste du système.

### 1. Installer les outils nécessaires

```bash
sudo apt install wget gpg apt-transport-https
```

### 2. Ajouter la clé de signature de Microsoft

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo install -D -o root -g root -m 644 microsoft.gpg /usr/share/keyrings/microsoft.gpg
rm microsoft.gpg
```

### 3. Déclarer le dépôt

```bash
sudo tee /etc/apt/sources.list.d/vscode.sources > /dev/null <<'EOF'
Types: deb
URIs: https://packages.microsoft.com/repos/code
Suites: stable
Components: main
Architectures: amd64,arm64,armhf
Signed-By: /usr/share/keyrings/microsoft.gpg
EOF
```

### 4. Installer VS Code

```bash
sudo apt update
sudo apt install code
```

!!! tip "Astuce"

    Les mises à jour de VS Code arrivent ensuite avec celles du système
    (`sudo apt update && sudo apt upgrade` ou la *Logithèque*).

## Accès aux ports série (ESP32, Arduino…) { #ports-serie }

Pour téléverser un programme sur une carte, l'utilisateur doit appartenir au groupe `dialout` :

```bash
sudo usermod -aG dialout $USER
```

!!! danger "Important"

    Fermez puis rouvrez la session pour que l'ajout au groupe soit pris en compte.
    Vérifiez ensuite avec la commande `groups`.
