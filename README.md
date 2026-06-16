#  Projet Platformer — Médiéval Fantasy
> Projet d'algorithmique CIN1/BIOST1 — Semestre 2, 2026  
> ISEN Méditerranée — 

---

##  Présentation

Jeu de plateforme en **langage C** pour Linux, développé avec la bibliothèque **Gfxlib** fournie par l'école.  
Le jeu s'inspire des platformers classiques (Mario, Hollow Knight) dans un univers **médiéval fantasy** : chevaliers, créatures et donjons

---

##  Équipe & Répartition

- **Mael Ritouet — Moteur Physique & Rendu Visuel**
  - **Fichiers clés :** `joueur.c`, `rendu.c`, `jeu.h`
  - **Responsabilités C :** Noyau dur algorithmique. Moteur physique (Coyote time, Jump buffer, collisions AABB), conversion de couleurs bit-à-bit (ARVB) pour la transparence, et conception des structures de données globales (`JeuEtat`).

- **Paul Gomez — Objets & Score**
  - **Fichiers clés :** `objets.c`
  - **Responsabilités C :** Gestion des entités interactives. Détection de collision (bounding boxes) entre le joueur et les pièces, gestion de la condition de victoire, et compteurs.

- **Luna Tardiveau — Menus & Fichiers de Sauvegarde**
  - **Fichiers clés :** `menu.c`, I/O Fichiers (dans `main.c`)
  - **Responsabilités C :** Logique d'interface utilisateur (navigation). Implémentation du système de sauvegarde des meilleurs temps via la manipulation de fichiers texte (`fopen`, `fprintf`, `fscanf`).

- **Aurélie Chodron de Courcel- Comportement Ennemis**
  - **Fichiers clés :** `ennemi.c`
  - **Responsabilités C :** Physique basique des ennemis (déplacements `vx/vy`, gravité simple), logique de patrouille et détection de collision avec les murs et les précipices.

- **Mathieu Cortes — Génération de Niveaux**
  - **Fichiers clés :** `blueprint.h`, `niveau.c`
  - **Responsabilités C :** Intégration algorithmique des niveaux. Écriture du parseur (`chargerNiveauDepuisBlueprint`) pour générer le monde à partir de matrices de caractères, et scrolling de la caméra.

---

## 🏗️ Architecture du projet

```
projet/
├── main.c              # Point d'entrée, événements clavier X11
├── jeu.h               # Constantes globales et structs de l'état du jeu
│
├── joueur.c / .h       # Physique, déplacements, collisions du joueur
├── niveau.c / .h       # Caméra et Parseur de matrices
├── blueprint.h         # Tableaux de caractères bruts des 9 niveaux
├── ennemi.c / .h       # Déplacements ennemis et collisions
├── objets.c / .h       # Collecte de pièces et condition de victoire
├── rendu.c / .h        # Affichage BMP (animations, transparence) et HUD
├── menu.c / .h         # Logique de l'écran titre et sélection
│
├── gfxlib/             # Bibliothèque fournie par l'ISEN
│   ├── GfxLib.c / .h
│   └── ...
│
└── images/             # Sprites .bmp 24-bit non compressés
```

---

## 🔄 Game Loop

La boucle de jeu repose sur `demandeTemporisation(16)` (~60 fps).  
À chaque tick :

```
Temporisation
    ├── mettreAJourJoueur()
    ├── mettreAJourCamera()
    ├── mettreAJourEnnemis()
    ├── mettreAJourObjets()
    └── rafraichisFenetre()
            └── Affichage → dessinerTout()
```

---

## 📦 Structures de données principales (`jeu.h`)

```c
// Constantes Principales
#define LARGEUR_FENETRE  1920
#define HAUTEUR_FENETRE  1080
#define GRAVITE          0.5f
#define VITESSE_JOUEUR   5.0f
#define FORCE_SAUT       10.5f
#define NB_PLATEFORMES   400
#define NB_ENNEMIS       80
#define NB_PIECES        200

// Joueur (Physique avancée)
typedef struct {
    float x, y, vx, vy;
    int direction;
    int vies, pieces;
    int invulnerable;
    int vivant, au_sol;
    int coyote_timer;
    int jump_buffer;
    int sauts_restants;
    int en_dash;
} Joueur;

// Ennemi (IA & Patrouille)
typedef struct {
    float x, y, vx, vy, vx_base;
    int direction, vivant, au_sol;
    int attaque, en_charge;
    int type_ennemi;
} Ennemi;

// État global du jeu
typedef struct {
    Joueur joueur;
    Plateforme plateformes[NB_PLATEFORMES];
    Ennemi ennemis[NB_ENNEMIS];
    Objet objets[NB_PIECES];
    int niveau_actuel, nb_niveaux;
    float camera_x, camera_y;
    int etat; // MENU, JEU, GAME_OVER, VICTOIRE...
} JeuEtat;
```

---

## ✅ Fonctionnalités implémentées

- [x] Architecture & structs définies
- [x] Déplacements joueur (gauche/droite)
- [x] Saut sur plateformes
- [x] Gravité & collisions
- [x] Ennemis fixes et mobiles
- [x] Orientation des sprites selon direction
- [x] Ennemis tués par saut ("stomp")
- [x] IA ennemis (attaque et patrouille)
- [x] Objets à collecter + bonus au seuil
- [x] HUD dynamique (niveau, timer, vies, pièces)
- [x] Représentation par sprites BMP 24-bits

## 🌟 Bonus réalisés

- [x] Physique avancée (Inertie, Coyote Time, Jump Buffer)
- [x] Double saut & mécanique de Dash
- [x] Frames d'invulnérabilité + son de dégât
- [x] Système dynamique de 9 niveaux (Parsing procédural)
- [x] Tableau de sauvegarde des records (I/O fichiers)
- [x] Animations fluides (plusieurs frames par état)
- [x] Caméra dynamique avec Parallaxe de fond
- [x] Textes flottants (Tutoriel intégré)

---

## 🎮 Contrôles du Jeu

- **Flèches Gauche/Droite** : Se déplacer
- **Barre Espace** : Sauter (et Double Saut dans les airs)
- **Touche Maj (Shift)** : Effectuer un Dash au sol ou en l'air
- **Touche Entrée** : Valider dans les menus
- **Échap** : Quitter le jeu

---

## 🔧 Compilation & Lancement

Avant de compiler, assurez-vous d'avoir les dépendances X11 et OpenGL (requises par GfxLib) :
```bash
sudo apt install libx11-dev libgl1-mesa-dev xorg-dev
```

```bash
make        # Compile le projet
./jeu       # Lance le jeu
make clean  # Nettoie les fichiers objets
```

> ⚠️ Le projet compile sous **Linux uniquement** avec `make`.  
---

## 📅 Historique du Développement

Plan de développement — A à Z
🟦 Phase 0 — Fondations (Jour 1 — 08/06)
Objectif : quelque chose s'affiche à l'écran, le projet compile.

Makefile — compilation de tous les .c en une commande
jeu.h — toutes les structs + constantes
main.c — fenêtre Gfxlib qui s'ouvre, boucle vide, fond noir
rendu.c — appels à traceRectangle() comme placeholder pour le joueur

✅ Critère de validation : une fenêtre noire s'ouvre avec un rectangle blanc qui représente le joueur.

🟨 Phase 1 — Joueur vivant (Jour 2 — 09/06)
Objectif : le joueur bouge et tombe.

joueur.c — gravité + vélocité verticale appliquée chaque tick
joueur.c — déplacement gauche/droite au clavier
niveau.c — sol hardcodé (une seule grande plateforme)
joueur.c — collision avec le sol (le joueur ne tombe pas dans le vide)
joueur.c — saut (impulsion vers le haut, une seule fois dans les airs)

✅ Critère : le joueur marche et saute sur un sol plat.

🟨 Phase 2 — Niveau jouable (Jour 3-4 — 10-11/06)
Objectif : un vrai niveau avec obstacles.

niveau.c — tableau de plateformes hardcodées (sol + plateformes en hauteur)
joueur.c — collisions avec toutes les plateformes (haut, bas, côtés)
niveau.c — précipices mortels (zones qui tuent le joueur)
niveau.c — caméra/scrolling horizontal (la vue suit le joueur en X)
rendu.c — affichage de toutes les plateformes

✅ Critère : un niveau scrollable avec des plateformes à plusieurs hauteurs, le joueur meurt s'il tombe.

🟧 Phase 3 — Ennemis (Jour 4-5 — 11-12/06)
Objectif : le niveau est dangereux.

ennemi.c — IA basique de patrouille (déplacement latéral, demi-tour au précipice/mur)
ennemi.c — collisions ennemi/joueur → dégâts et frames d'invulnérabilité
ennemi.c — mécanique de Stomp → saut sur la tête de l'ennemi pour le vaincre
rendu.c — animation cyclique (plusieurs frames BMP) des slimes et démons

✅ Critère : des ennemis patrouillent, on peut les tuer en sautant dessus.

🟧 Phase 4 — Objets & Game Logic (Jour 5-6 — 12-13/06)
Objectif : le jeu a un but.

objets.c — algorithme de ramassage des pièces (bounding box)
objets.c — compteurs de score et validation de la condition de victoire
main.c — machine à états (`JeuEtat`) : Menu → Jeu → GameOver → Victoire
menu.c — navigation au clavier de l'écran titre et sélection du niveau

✅ Critère : on peut gagner ou perdre, le jeu a un début et une fin.

🟥 Phase 5 — Sprites BMP & Rendu (Jour 6-7 — 13-14/06)
Objectif : le jeu ressemble à quelque chose.

rendu.c — fonction `chargerSprite()` : parsing BMP et correction manuelle ARVB pour la transparence
rendu.c — intégration des assets Medieval Fantasy sur toutes les hitboxes
rendu.c — HUD complet avec typographie vectorielle : vies, timer, pièces
rendu.c — gestion de la direction (chargement séparé des assets gauche/droite, palliant l'absence de flip horizontal de GfxLib)

✅ Critère : le jeu est visuellement identifiable comme un platformer médiéval.

🟩 Phase 6 — Polish & bonus (Jour 8-9 — 15-17/06)
Objectif : se démarquer à la soutenance.

Physique avancée — Implémentation du Coyote Time et du Jump Buffer
Gameplay — Ajout du Double Saut et du Dash multidirectionnel
Niveaux — Création du parseur procédural pour charger les 9 niveaux depuis `blueprint.h`
Visuel — Correction de la transparence ARVB et parallaxe fonctionnel

✅ Critère : le jeu est fluide, complet et techniquement complexe.

🏁 Phase 7 — Livraison (Jour 10 — 18/06)
Objectif : Release finale.

Compilation stable sous Linux
Préparation de la soutenance

---

## 🎨 Crédits & Assets

- **Moteur de base** : `GfxLib` (fournie par l'ISEN).
- **Sprites & Textures** : Modifiés et adaptés au format BMP 24-bits depuis des banques libres de droits (itch.io).
- **Effets sonores** : Générés via `aplay` (WAV PCM 8/16 bits).
