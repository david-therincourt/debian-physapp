# Oscilloscope

## Siglent : enregistrer les fichiers sur l'ordinateur par le réseau

Les oscilloscopes Siglent (SDS2104X…) peuvent enregistrer leurs captures d'écran et leurs
fichiers de mesures directement dans un **dossier partagé** sur le réseau (*Net Storage*).
L'ordinateur partage un dossier avec **Samba** et l'oscilloscope s'y connecte comme un lecteur réseau.

Dans l'exemple ci-dessous, le compte utilisateur de l'ordinateur est `eleve`
et le dossier partagé s'appelle `SDS2104X`.

### 1. Installer Samba et créer le dossier

```bash
sudo apt install samba
mkdir ~/SDS2104X
```

### 2. Déclarer le partage

Ajoutez le partage à la fin du fichier de configuration `/etc/samba/smb.conf` :

```bash
sudo tee -a /etc/samba/smb.conf > /dev/null <<'EOF'

[SDS2104X]
   comment = Oscilloscope Siglent
   path = /home/eleve/SDS2104X
   read only = no
   browsable = yes
EOF
```

Vérifiez que la configuration ne contient pas d'erreur :

```bash
testparm -s
```

### 3. Créer le mot de passe Samba de l'utilisateur

Samba utilise ses propres mots de passe, distincts de celui de la session :

```bash
sudo smbpasswd -a eleve
```

### 4. Démarrer Samba

```bash
sudo systemctl enable --now smbd
sudo systemctl restart smbd
sudo systemctl status smbd
```

Samba démarrera ensuite automatiquement avec l'ordinateur.

### 5. Relever l'adresse IP de l'ordinateur

```bash
hostname -I
```

### 6. Configurer l'oscilloscope

Dans le menu **Utility → Net Storage** de l'oscilloscope :

| Paramètre   | Valeur                                |
|:------------|:--------------------------------------|
| Drive       | `I:`                                  |
| Directory   | `//<adresse IP du PC>/SDS2104X`       |
| Username    | `eleve`                               |
| Password    | le mot de passe Samba de l'étape 3    |

Les fichiers enregistrés sur le lecteur `I:` de l'oscilloscope apparaissent
directement dans le dossier `~/SDS2104X` de l'ordinateur.

!!! tip "Remarque"

    Si l'ordinateur a plusieurs cartes réseau, vous pouvez limiter Samba à l'une d'elles en
    ajoutant dans la section `[global]` de `smb.conf` :
    `interfaces = enp4s0` et `bind interfaces only = yes` (remplacez `enp4s0` par le nom
    de l'interface, donné par la commande `ip link`).

!!! warning "Attention"

    Si l'oscilloscope n'arrive pas à se connecter, il ne gère peut-être que l'ancien protocole SMB1,
    désactivé par défaut. Ajoutez alors `server min protocol = NT1` dans la section `[global]`
    de `smb.conf`, puis redémarrez Samba. Ce protocole est ancien et peu sûr : à réserver
    à un réseau de salle de TP.
