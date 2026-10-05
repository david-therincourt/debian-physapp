# Multimédia

## Codecs audio et vidéo

Les **codecs** permettent de lire les formats audio et vidéo courants (MP4, H.264, H.265, MP3, AAC…)
dans les applications GNOME : lecteur vidéo, aperçu de Nautilus, navigateur…

```bash
sudo apt install gstreamer1.0-libav gstreamer1.0-plugins-good \
                 gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly \
                 ffmpeg libavcodec-extra
```

| Paquet                          | Rôle                                                   |
|:--------------------------------|:-------------------------------------------------------|
| `gstreamer1.0-libav`            | Décodage de la plupart des formats (H.264, AAC…) via FFmpeg |
| `gstreamer1.0-plugins-good`     | Formats libres et courants (MP3, FLAC, WebM…)          |
| `gstreamer1.0-plugins-bad`      | Formats plus récents ou moins éprouvés (H.265…)         |
| `gstreamer1.0-plugins-ugly`     | Formats soumis à des brevets (MPEG-2…)             |
| `ffmpeg`                        | Outil de conversion audio et vidéo en ligne de commande |
| `libavcodec-extra`              | Codecs supplémentaires pour FFmpeg                      |

## VLC

**VLC** est un lecteur multimédia qui lit presque tous les formats audio et vidéo,
ainsi que les flux réseau. Il intègre ses propres codecs.

```bash
sudo apt install vlc vlc-l10n
```

!!! tip "Astuce"

    Pour faire de VLC le lecteur par défaut : **Paramètres → Applications → Applications par défaut**,
    puis choisir VLC pour la musique et la vidéo.
