# Arduino-LMS-portable

Environnement Arduino **entièrement portable**, avec VSCode et Arduino-CLI.

## 📦 Téléchargement de l’environnement portable

Le lien vers le fichier complet (archive `.7z`) est disponible dans la section **Releases** du dépôt :

👉 [https://github.com/fleb72/Arduino-LMS-portable/releases](https://github.com/fleb72/Arduino-LMS-portable/releases)

Seule la **dernière version** est conservée sur le serveur (espace limité).

## IMPORTANT — INSTALLATION
Arduino LMS portable ~~existe maintenant en deux variantes~~ est désormais proposé uniquement en **version light**, optimisée pour les usages pédagogiques en lycée.

La version *full* (incluant l’ensemble des cores pour Arduino et ESP32) a été retirée et est considérée comme **obsolète** : trop volumineuse, peu utilisée, et non adaptée aux contraintes des postes élèves.

La **Version light** est un pack allégé (~2,3 Go) qui contient les *cores* suivants :

| ID                   | Installed | Name                                           |
|----------------------|-----------|------------------------------------------------|
| arduino:avr          | 1.8.8     | Arduino AVR Boards                             |
| arduino:renesas_uno  | 1.6.0     | Arduino UNO R4 Boards                          |

Cette version est idéale pour les établissements scolaires ou les salles informatiques où seules les cartes Arduino officielles sont utilisées. Et il est toujours possible d'y installer facilement des cores supplémentaires (ESP32, etc.)

---
Le dossier est archivé avec 7zip (gratuit, extension .7z).
Prévoyez suffisamment d’espace disque, un temps d'extraction et de copie adapté (clé USB lente = copie longue). La première tâche avant déploiement consistera à installer ou supprimer des cores selon vos matériels disponibles.

Pour que cet environnement fonctionne correctement, vous devez copier le dossier complet "Arduino-LMS-portable" à la racine du disque C:, de manière à obtenir :
`C:\Arduino-LMS-portable\`

Ne pas placer ce dossier dans "Documents", "Téléchargements", "Desktop", un réseau, ou un sous-dossier.  
Le chemin C:\ est indispensable pour le bon fonctionnement des tâches VS Code et de l’arduino-cli.

Une fois copié à cet emplacement, tout fonctionne immédiatement.


## LANCER L’ENVIRONNEMENT (IMPORTANT)

Cet environnement utilise **une version portable de [VS Code](https://code.visualstudio.com/Download)** (v1.125.0), déjà configurée pour Arduino CLI.

Vous devez OBLIGATOIREMENT lancer VS Code via :
`C:\Arduino-LMS-portable\code.exe - Raccourci`

Ne pas utiliser un VS Code installé ailleurs sur le PC : il ne contient pas les extensions, les chemins, ni les tâches nécessaires au fonctionnement du pack portable.

Une fois VS Code lancé depuis `code.exe`, ouvrez vos sketches (menu File → Open Folder…) dans :
`C:\Arduino-LMS-portable\arduino-home\sketches\`


## CONTENU DU DOSSIER

```text
C:\Arduino-LMS-portable\
│
├── arduino-cli\
│     ├── arduino-cli.exe
│     ├── arduino-cli.yaml
│     └── data\
│           └── packages\
│                ├── arduino\              → Core UNO + UNO R4 WiFi
│                ├── Heltec-esp32\         → Core Heltec WiFi LoRa 32 V3 (sauf version 'light')
│                └── esp32\                → Core ESP32-WROOM / DevKitC (sauf version 'light')
│
└── arduino-home\
      ├── sketches\	
      │     ├── _tests\
      │     │      ├── uno_avr_test\
      │     │      ├── uno_r4_test\
      │     │      └── ...
      │     └── projets (vos futurs sketches ici)
      │
      ├── libraries\
```

## CORES PRÉINSTALLÉS

- Arduino AVR (`arduino:avr`)  
- Arduino Renesas UNO (`arduino:renesas_uno`)  


## BIBLIOTHÈQUES PRÉINSTALLÉES

Les bibliothèques installées se trouvent dans :
    `C:\Arduino-LMS-portable\arduino-home\libraries\`


## COMPILER UN PROGRAMME (BUILD) — via VS Code

Chaque sketch possède son propre dossier :
`C:\Arduino-LMS-portable\arduino-home\sketches\<nom_du_sketch>\.vscode\`

Ce dossier contient :
- `tasks.json`  → build / upload automatisés
- `settings.json` → port COM spécifique au sketch

### Pour compiler un sketch :

1. Ouvrir le dossier du sketch dans VS Code  
2. Aller dans :  
       Terminal → Run Task…  
3. Choisir :  
       `Arduino Build`  
4. Le binaire est généré dans :  
       `./build/`

Raccourci : **Ctrl + Shift + B**

## Pourquoi utiliser arduino-cli ?

Initialement, parce que les cores ESP32 pour Arduino sont très volumineux. 
Lors de la première compilation, l’outil doit construire l’intégralité du core (FreeRTOS, WiFi, Bluetooth, drivers, etc.). Cela peut prendre **jusqu’à 40 minutes** selon la machine.

Bonne nouvelle : tous les fichiers compilés sont ensuite stockés dans le dossier `/build` du projet. Lors des compilations suivantes, Arduino réutilise ce cache et ne recompile que le strict nécessaire.

Résultat → la seconde compilation prendra moins d'une minute.

Conseil : ne supprimez pas le dossier `/build` si vous voulez conserver des compilations rapides.

[Arduino-cli](https://docs.arduino.cc/arduino-cli/) a été choisi, car il offre une expérience plus rapide et plus fiable que l’IDE Arduino 1.x et 2.x, surtout pour les cartes ESP32. 
Après une première compilation longue, arduino-cli réutilise un cache local et réduit les compilations suivantes à moins d'une minute. L’environnement est portable, reproductible et fonctionne sans installation, ce qui est idéal pour un usage en classe ou sur des PC verrouillés.

## CONFIGURER LE PORT COM (OBLIGATOIRE AVANT UPLOAD)

Le port COM doit être défini dans le fichier :
`C:\Arduino-LMS-portable\arduino-home\sketches\<sketch>\.vscode\settings.json`

Exemple :
```json
{
    "arduino.port": "COM5"
    ...
}
```
Chaque sketch peut avoir son propre port COM.


## TROUVER LE PORT COM AVEC ARDUINO-CLI


Brancher la carte USB puis dans VSCode :
	Terminal → Run Tasks → `Arduino : list COM ports`

Exemple de résultat :

|Port|     Protocol|  Board Name|
|----|-------------|------------|
|COM5|     serial  |  Arduino Uno R4 WiFi|

→ Le port est ici **COM5**  
→ Le reporter dans le `settings.json` du sketch.


## TÉLÉVERSER VIA VS CODE

Une fois le port COM configuré :

1. Aller dans :  
       Terminal → Run Task…  
2. Choisir :  
       `Arduino Upload`


## COMPILER + TÉLÉVERSER AUTOMATIQUEMENT
Terminal → Run Task… → `Arduino: Build + Upload`

Cette tâche enchaîne compilation → upload.


## INSTALLER UNE NOUVELLE BIBLIOTHÈQUE (ZIP)

1. Télécharger le ZIP  
2. Décompresser  
3. Renommer le dossier (ex : ArduinoJson)  
4. Copier dans :  
       `C:\Arduino-LMS-portable\arduino-home\libraries\`


## INSTALLER UN NOUVEAU CORE (CARTE) / DÉSINSTALLER

Les commandes `arduino-cli` ne fonctionnent QUE dans le terminal intégré de VSCode (menu Terminal → New Terminal).

Pour lister les cores déjà installés :

```
arduino-cli --config-file C:\Arduino-LMS-portable\arduino-cli\arduino-cli.yaml core list
```

Pour installer/désinstaller un core :

1. Ajouter l’URL du core dans :
   ```
       C:\Arduino-LMS-portable\arduino-cli\arduino-cli.yaml
   ```

2. Mettre à jour l’index :
    ```
    arduino-cli --config-file C:\Arduino-LMS-portable\arduino-cli\arduino-cli.yaml core update-index
    ```

3. Installer le core :
   ```
   arduino-cli --config-file C:\Arduino-LMS-portable\arduino-cli\arduino-cli.yaml core install <fabricant:core>
   ```

4. Pour désinstaller un core :
   ```
	arduino-cli --config-file C:\Arduino-LMS-portable\arduino-cli\arduino-cli.yaml core uninstall <fabricant:core>
   ```


## TESTS FOURNIS

Les sketches de test se trouvent dans : 
`C:\Arduino-LMS-portable\arduino-home\sketches\_tests\`

Tests inclus :
- `uno_avr_test\`       → Test UNO  
- `uno_r4_test\`        → Test UNO R4 WiFi  
- `lorawan_test\`       → Test Heltec LoRa V3 (ne compilera pas avec la version 'light')
- `esp32_test\`		    → Test ESP32 dev module (ne compilera pas avec la version 'light')

### Structure des sketches de test : .ino vide + code en .cpp

Chaque dossier contient :

- un fichier `.ino` **vide**  
- un fichier `.cpp` contenant **tout le code réel**

### Pourquoi cette structure ?

Arduino CLI **exige obligatoirement** la présence d’un fichier `.ino` pour considérer un dossier comme un "sketch Arduino".

Mais :

- VS Code fonctionne beaucoup mieux avec un `.cpp` (analyse, autocomplétion…)    
- Le build compile **tous les .cpp** du dossier automatiquement

Ici, le `.ino` vide sert uniquement de **placeholder** pour Arduino CLI.

Ces sketchs peuvent servir aussi de *template* pour vos futurs projets.


## UTILISATION EN ÉTABLISSEMENT SCOLAIRE

- aucun droit administrateur
- aucun accès Internet  
- aucun fichier dans `C:/users/.../AppData`  
- compatible salles informatiques verrouillées  


## POURQUOI CE PROJET EXISTE

Ce pack portable a été conçu pour répondre aux difficultés rencontrées dans les établissements scolaires :

- Les compilations lentes ou impossibles  
  → Les réseaux des lycées filtrent fortement, ce qui ralentit
    ou bloque les milliers de petits fichiers nécessaires aux
    toolchains Arduino (AVR, ESP32, Renesas…).  
  → Les antivirus scolaires analysent chaque fichier, ce qui
    multiplie les temps de compilation par 5 ou 10.

- Les droits d’accès très limités  
  → Impossible d’installer Arduino IDE ou VS Code.  
  → Impossible d’écrire dans AppData ou Program Files.  
  → Impossible d’installer Python, Git, les drivers, etc.

- Les dépendances qui ne se téléchargent pas  
  → Les serveurs GitHub, Python.org, AmazonAWS ou Espressif
    sont souvent bloqués par les proxys académiques.  
  → Résultat : PlatformIO ou Arduino IDE ne peuvent pas
    installer les cores, les frameworks ou les toolchains.

- Les environnements qui cassent d’une salle à l’autre  
  → Un PC autorise l’installation, un autre non.  
  → Un antivirus bloque un fichier, un autre le laisse passer.  
  → Les élèves n’ont jamais le même environnement.

Ce pack portable résout ces problèmes :

- Aucun droit administrateur  
- Aucun accès Internet  
- Aucun fichier dans AppData  
- Aucun téléchargement de dépendances  
- Tout est préinstallé et reproductible  
- Fonctionne dans toutes les salles, sur tous les PC  
- Copie → Lancez code.exe → Ça marche

Ce projet a été créé pour permettre aux enseignants et aux élèves de travailler sereinement, même dans un environnement informatique très contraint.

## CHANGELOG

#### v1 — 2026‑06‑26 (Obsolète)
- Version initiale d'environnement portable Arduino/ESP32

#### v1.0.1 — 2026-06-28 (Obsolète)
- Suppression de l’extension C/C++ de Microsoft
- Amélioration de la portabilité
- Aucun changement dans le fonctionnement du pack

Si vous utilisez déjà la version 1.0, vous n'avez pas besoin de re‑télécharger les 6 Go.
Il suffit de supprimer l’extension C/C++ dans VS Code :
1. Ouvrir VS Code
2. Aller dans Extensions
3. Rechercher "C/C++"
4. Cliquer sur "Uninstall"

Votre pack devient identique à la version 1.0.1.

#### v1.0.1 light - 2026-06-29
- Version allégée (*light*) ne comprenant que les *cores* pour Arduino AVR (Uno/Nano/Mega) et Arduino Renesas (Unor R4 Minima/WiFi).


#### v1.1 light - 2026-06-30

**Deux nouvelles tâches disponibles dans Terminal -> Run task… :**

- Créer un projet UNO/Nano/Mega (AVR)
- Créer un projet UNO R4 Minima/WiFi (Renesas)
Dans les deux cas, le nom du projet est à renseigner
Une fois créé, le projet de destination est dans `C:\Arduino-LMS-portable\arduino-home\sketches\projets\<Nom du projet>`

Chaque projet est prêt à compiler et téléverser via les tâches Arduino habituelles.

\---

**Améliorations techniques**

-  Intégration de **Node.js portable** dans `tools/nodejs/` (aucune installation requise).
-  Script `copy-template.js` :

  * Vérifie l’existence du dossier avant copie.
  * Copie le template depuis `..\arduino-home\sketches\_tests` vers `..\arduino-home\sketches\projets`.
  * Renomme automatiquement les fichiers `.ino` et `.cpp`.
  * Affiche un message de confirmation dans le Terminal (mode `"reveal": "always"`).

`text 
Arduino-LMS-portable/
├── arduino-cli/
├── arduino-home/
│    ├── sketches/
│    │    ├── _tests/
│    │    └── projets/
├── tools/
│    ├── nodejs/
│    └── copy-template.js
└── VSCode/
└── data/user-data/User/tasks.json
`

**Fix**
- Correction de l’apparition du sous‑menu "Continue without scanning the task output" lors de l’exécution des tâches VS Code (`problemMatcher: []` ajouté).

