# tb-data

Données expérimentales du Travail de Bachelor :

**Dans quelle mesure l’exécution d’upscaling vidéo par IA côté client permet-elle de réduire la consommation énergétique, la taille et les coûts des data centers / cloud par rapport à un rendu traditionnel serveur ?**

- Auteur : Adriano José Queirós da Silva
- École : Haute École de Gestion de Genève (HEG-GE), filière Informatique de gestion
- Conseiller : Athanasios Priftis
- Date : Genève, 18 septembre 2026

Ce dépôt publie les **chiffres bruts** et les **classeurs Excel mis en page** utilisés dans le mémoire : mesures de puissance côté client, scores objectifs (VMAF, PSNR, SSIM, LPIPS, ERQA) et export du test subjectif Subjectify.us.

Licence des données et des Excel : **[CC BY 4.0](LICENSE)**.  
Citation : Queirós da Silva, A. J. (2026). Travail de Bachelor, HEG-GE. Voir aussi [`CITATION.cff`](CITATION.cff).

---

## Contenu

```
01-QUAL/                          qualité des reconstructions
  01-LOS/                         scores full-reference, corpus lossless
    01-VMAFV1/                    contrôle VMAF v1 (même clips lossless)
  02-LAD/                         ladder CRF / « LAD »
  03-ADP/                         ladder adaptatif (Sunflower, Karting)
  04-metric-atelier/              exports de l’outil metric-atelier
  05-subjectify/                  test par paires Native 4K vs VSR
    01-Brute/ai-upscaling.csv     export brut Subjectify.us
    Subjectify-VSR-vs-Native4K.xlsx
02-Power/                         consommation électrique client
  Laptops/
    Cout_AI_Laptops.xlsx          synthèse MacBook Air M4 / Surface Laptop 4
    numbers-logger/macairm4-W/    CSV OCR wattmètre, M4
    numbers-logger/surfacelp4-W/  CSV OCR wattmètre, Surface
  NVIDIA/
    NVIDIA-POWERCONSUMPTION.xlsx  synthèse tour (4080 Super / 2070)
    2070/Consomation-GPU-VSR-CHROME-2070.xlsx
    4080s/Consommation-MPV-4080s.xlsx
    4080s/Consommation-VSR-CHROME-4080.xlsx
    4080s/numbers-logger/         CSV sessions tour
```

### Comment lire les CSV `numbers-logger`

Colonnes : `timestamp`, `value` (watts lus à l’écran), `raw` (texte OCR), `region` (zone capturée).  
Convention de nommage portable :

| Jeton | Signification |
| --- | --- |
| `bbb` / `mer` / `bea` / `sol` | Big Buck Bunny (Sunflower), Meridian, Beauty, Sol Levante |
| `360p` `720p` `1080p` `4k` | résolution d’entrée |
| `24` / `60` | images par seconde |
| `ai2x` | FSRCNNX 2× actif |
| `noai` | lecture sans réseau |
| `idle` | plancher machine au repos |

Sur la tour, les fichiers `numbers-YYYY-MM-DD_HH-MM-SS.csv` sont des sessions horodatées ; le mapping session → condition est dans les classeurs Excel du dossier `NVIDIA/`.

### Comment lire `01-QUAL`

- `01-LOS` : reconstructions depuis un échelon lossless, comparées à la référence 4K.
- `02-LAD` : même comparaison après quantification CRF.
- `03-ADP` : ladder adaptatif (bitrate type plateforme).
- `05-subjectify` : 78 sessions validées, paires Native4K contre VSR depuis 360p / 480p / 720p / 1080p.

Les CSV d’import sous `04-metric-atelier/data/imports/` sont des copies de travail de l’outil ; les fichiers de `01-LOS`, `02-LAD` et `03-ADP` font foi.

---

## Chaîne expérimentale (autres dépôts)

Outils utilisés pour produire ces fichiers :

| Dépôt | Rôle |
| --- | --- |
| [tb-ffmpeg-ladder](https://github.com/DaSilva-Adriano/tb-ffmpeg-ladder) | Ladder FFmpeg 4K / 1080p / 720p / 480p / 360p, cut lossless, extend |
| [MP4ADPLADDER](https://github.com/DaSilva-Adriano/MP4ADPLADDER) | Ladder adaptatif x265 / MP4 |
| [noai-classic-upscale](https://github.com/DaSilva-Adriano/noai-classic-upscale) | Baselines bicubique / Lanczos (sans IA) |
| [rtx-vsr-lab](https://github.com/DaSilva-Adriano/rtx-vsr-lab) | Dumps RTX Video Super Resolution (SDK) |
| [clientsr-dump-lab](https://github.com/DaSilva-Adriano/clientsr-dump-lab) | Dumps 2× FSRCNNX / Anime4K / AnimeJaNai |
| [fsrcnnx-player](https://github.com/DaSilva-Adriano/fsrcnnx-player) | Lecture temps réel FSRCNNX-8 / FSRCNNX-16 (mpv) |
| [numbers-logger](https://github.com/DaSilva-Adriano/numbers-logger) | OCR d’un wattmètre / overlay et journal CSV |
| [sr-video-lab](https://github.com/DaSilva-Adriano/sr-video-lab) | Inspection visuelle locale des reconstructions |
| [vsr-eval](https://github.com/DaSilva-Adriano/vsr-eval) | Métriques full-reference (PSNR, SSIM, MS-SSIM, VMAF, LPIPS, ERQA) |
| [metric-atelier](https://github.com/DaSilva-Adriano/metric-atelier) | Import, regroupement et comparaison des CSV de métriques |

Profil : [github.com/DaSilva-Adriano](https://github.com/DaSilva-Adriano)

---

## Plateformes mesurées (`02-Power`)

- GeForce RTX 4080 Super (tour, secteur)
- GeForce RTX 2070 (effet générationnel)
- MacBook Air M4
- Microsoft Surface Laptop 4

Méthodes d’upscaling concernées par ces séries : RTX Video Super Resolution (Chrome) et FSRCNNX 2× (mpv).

---

## Corpus vidéo (non inclus ici)

Les masters et extraits restent chez leurs ayants droit. Ne pas rellicencier ces médias.

- Meridian, Sol Levante, Sparks — Netflix Open Content
- Big Buck Bunny / Sunflower, Tears of Steel — Blender Foundation
- Beauty, Karting, Jockey, Floorball, RaceNight — Ultra Video Group

---

## Réutilisation

Les données et les classeurs Excel de ce dépôt sont sous [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).  
Le code des outils listés plus haut a sa propre licence (MIT, GPL, etc.) dans chaque dépôt.
