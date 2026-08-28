# Walky

Device de streaming audio indépendant, portable et compact (objectif : taille téléphone).

## Vision

- Écoute filaire ou BLE, à partir de fichiers audio préinstallés ou de services de streaming (Spotify, Deezer...)
- Fonctionne **online** (streaming) ou **offline** (musique téléchargée à l'avance) — usage **majoritairement hors-ligne**
- Autonomie minimum visée : **8h en mode hors-ligne**
- Pas d'écran tactile : navigation via 2 molettes (haut/bas + gauche/droite) + 1 bouton de sélection

## Hardware / OS

- **Cible finale** : processeur x86 (LattePanda IOTA)
- **Hardware de dev actuel** : Raspberry Pi 3 (en attendant le portage x86)
- **OS** : Linux personnalisé via **Yocto** (Scarthgap)
- **Pourquoi x86** : besoin d'installer le vrai client Spotify (pas `librespot`, qui ne permet pas la sauvegarde offline)
- **Stack** : systemd, Weston en kiosk mode, Qt6/QML pour les UIs

## Musique — deux cas d'usage

- **Bibliothèque perso (offline)** : fichiers déposés sur carte SD, indexés par **MPD**, piloté par un client custom léger (molettes)
- **Streaming** : app officielle du service (Spotify/Deezer) installée sur Walky, UI native de l'app

## UX runtime

- **Pas d'app mobile dédiée** : setup/reconfig via **captive portal** classique (type Chromecast/Sonos)
- **AP wifi à la demande** (appui long), pas d'AP permanent, pour préserver l'autonomie
- **Pairing téléphone** via QR code affiché à l'écran : sert soit à la config wifi, soit de **clavier déporté** pour saisir du texte dans l'app affichée sur Walky
- Sécurité sessions : token éphémère à usage unique, expiration courte, réseau local uniquement

## Provisioning usine — Calamares

- Rôle réduit à un **outil usine uniquement** (jamais vu par l'utilisateur final)
- Tous les modules interactifs tournent en silencieux (`exec`) avec valeurs par défaut
- Config versionnée dans `walky-calamares-config/`, testée en VM (VirtualBox + shared folder)

## État d'avancement (26/08/2026)

### ✅ Phase préliminaire Yocto — validée
Image Scarthgap bootable sur RPi3, autologin, Weston en kiosk mode lancé via `.profile`, `weston.service`/`weston.socket` désactivés proprement.

### 🔜 Prochaines étapes
- Valider une vraie app Qt6/QML hello-world plein écran sur le kiosk
- Portage vers le hardware cible x86 (LattePanda IOTA)
- Développement des briques Walky : daemon uinput molettes (C), client MPD (Python/mpc)
- Détail du protocole de pairing QR, OAuth device flow Spotify/Deezer
- Repo Calamares (config usine détaillée)
