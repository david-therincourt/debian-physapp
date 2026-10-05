# LaTeX

## Installation minimale pour Pandoc

**Pandoc** convertit des documents d'un format à un autre (Markdown, Word, HTML…).
Pour produire un **PDF**, il s'appuie sur LaTeX. Inutile d'installer TeX Live complet
(`texlive-full`, plusieurs gigaoctets) : quelques paquets suffisent.

```bash
sudo apt install pandoc texlive-latex-base texlive-latex-recommended \
                 texlive-latex-extra texlive-fonts-recommended texlive-lang-french
```

| Paquet                      | Rôle                                                     |
|:----------------------------|:---------------------------------------------------------|
| `pandoc`                    | Le convertisseur                                         |
| `texlive-latex-base`        | Moteur `pdflatex` et classes de base                     |
| `texlive-latex-recommended` | Extensions courantes (`geometry`, `hyperref`, `microtype`…) |
| `texlive-latex-extra`       | Extensions utilisées par le modèle de Pandoc (`parskip`, `footnotehyper`, `xurl`…) |
| `texlive-fonts-recommended` | Polices (Latin Modern, Times, Helvetica…)                |
| `texlive-lang-french`       | Règles typographiques françaises (`babel-french`)        |

### Moteur XeLaTeX (facultatif)

Pour utiliser les polices installées sur le système (`mainfont`) et mieux gérer l'Unicode :

```bash
sudo apt install texlive-xetex
```

## Exemple d'utilisation

Conversion d'un fichier Markdown en PDF, avec la typographie française :

```bash
pandoc document.md -o document.pdf -V lang=fr -V geometry:margin=2.5cm
```

Avec XeLaTeX et une police du système :

```bash
pandoc document.md -o document.pdf --pdf-engine=xelatex \
       -V lang=fr -V mainfont="DejaVu Serif"
```

!!! tip "Astuce"

    Si une conversion échoue avec un message `File 'xxx.sty' not found`, cherchez le paquet
    qui contient ce fichier avec `apt-file search xxx.sty` (après `sudo apt install apt-file`
    et `sudo apt-file update`).
