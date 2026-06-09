# ⚔️ Projet Platformer — Médiéval Fantasy
> Projet d'algorithmique CIN1/BIOST1 — Semestre 2, 2026  
> ISEN Méditerranée — Groupe : *[numéro à renseigner]*

---

## 🗂️ Présentation

Jeu de plateforme en **langage C** pour Linux, développé avec la bibliothèque **Gfxlib** fournie par l'école.  
Le jeu s'inspire des platformers classiques (Mario, Hollow Knight) dans un univers **médiéval fantasy** : chevaliers, créatures, donjons et trésors.

---

## 👥 Équipe & Répartition

| Membre | Rôle |
|---|---|
| *[Prénom NOM]* | Moteur physique & joueur |
| *[Prénom NOM]* | Niveau & caméra |
| *[Prénom NOM]* | Ennemis |
| *[Prénom NOM]* | Objets & game logic |
| *[Prénom NOM]* | Rendu graphique & HUD |

---

## 🏗️ Architecture du projet

```
projet/
├── main.c              # Point d'entrée, boucle événementielle (Gfxlib)
├── jeu.h               # Structs partagées + constantes globales
│
├── joueur.c / .h       # Physique, déplacements, collisions joueur
├── niveau.c / .h       # Plateformes, précipices, scrolling caméra
├── ennemi.c / .h       # IA ennemis, attaque, mort
├── objets.c / .h       # Pièces/items, powerups, game logic
├── rendu.c / .h        # Affichage sprites BMP, HUD
│
├── gfxlib/             # Bibliothèque fournie (non modifiée)
│   ├── GfxLib.c / .h
│   ├── BmpLib.c / .h
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
    ├── mettreAJourEnnemis()
    ├── mettreAJourObjets()
    ├── verifierCollisions()
    └── rafraichisFenetre()
            └── Affichage → dessinerTout()
```

---

## 📦 Structures de données principales (`jeu.h`)

```c
// Constantes
#define LARGEUR_FENETRE  1280
#define HAUTEUR_FENETRE  720
#define GRAVITE          0.5f
#define VITESSE_JOUEUR   4.0f
#define FORCE_SAUT       12.0f
#define NB_PLATEFORMES   30
#define NB_ENNEMIS       8
#define NB_PIECES        20

// Joueur
typedef struct {
    float x, y, vx, vy;
    int direction;      // 1=droite, -1=gauche
    int vies;
    int pieces;
    int invulnerable;   // timer en frames
    int vivant;
} Joueur;

// Plateforme
typedef struct {
    float x, y, largeur, hauteur;
    int mortelle;       // 1=précipice/spike
} Plateforme;

// Ennemi
typedef struct {
    float x, y, vx;
    int direction;
    int vivant;
    int attaque;
} Ennemi;

// Objet collectible
typedef struct {
    float x, y;
    int collectee;
    int type;           // 0=pièce, 1=powerup...
} Objet;

// État global du jeu
typedef struct {
    Joueur joueur;
    Plateforme plateformes[NB_PLATEFORMES];
    Ennemi ennemis[NB_ENNEMIS];
    Objet objets[NB_PIECES];
    int niveau_actuel;
    float timer;
    int etat;           // 0=menu, 1=jeu, 2=game_over, 3=victoire
} JeuEtat;
```

---

## ✅ Fonctionnalités obligatoires

- [x] Architecture & structs définies
- [ ] Déplacements joueur (gauche/droite)
- [ ] Saut sur plateformes
- [ ] Gravité & collisions
- [ ] Ennemis fixes et mobiles
- [ ] Orientation des sprites selon direction
- [ ] Ennemis tués par saut ou attaque
- [ ] IA ennemis (attaque si joueur proche)
- [ ] Objets à collecter + bonus au seuil
- [ ] HUD (niveau, timer, vies, pièces)
- [ ] Représentation par sprites BMP

## 🌟 Bonus envisagés

- [ ] Inertie des déplacements
- [ ] Frames d'invulnérabilité + clignotement
- [ ] Jump pads
- [ ] Plusieurs niveaux
- [ ] Tableau des scores (fichier)
- [ ] Animations (plusieurs frames par sprite)
- [ ] Parallaxe arrière-plan

---

## 🔧 Compilation & Lancement

```bash
make        # Compile le projet
./jeu       # Lance le jeu
make clean  # Nettoie les fichiers objets
```

> ⚠️ Le projet compile sous **Linux uniquement** avec `make`.  
> Ne pas inclure les `.o` et l'exécutable dans l'archive de rendu.

---

## 📅 Planning

Plan de développement — A à Z
🟦 Phase 0 — Fondations (Jour 1 — 08/06)
Objectif : quelque chose s'affiche à l'écran, le projet compile.

Makefile — compilation de tous les .c en une commande
jeu.h — toutes les structs + constantes
main.c — fenêtre Gfxlib qui s'ouvre, boucle vide, fond noir
rendu.c — fonction dessinerRectangle() placeholder pour le joueur

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

ennemi.c — ennemi qui se déplace latéralement, fait demi-tour au bord
ennemi.c — collision ennemi/joueur → le joueur perd une vie
ennemi.c — détection joueur proche → l'ennemi "charge"
joueur.c — saut sur ennemi → l'ennemi meurt
rendu.c — orientation du sprite joueur + ennemi selon direction

✅ Critère : des ennemis patrouillent, on peut les tuer en sautant dessus.

🟧 Phase 4 — Objets & game logic (Jour 5-6 — 12-13/06)
Objectif : le jeu a un but.

objets.c — pièces placées dans le niveau, disparaissent au contact
objets.c — compteur de pièces → bonus au seuil (ex : +1 vie à 10 pièces)
objets.c — condition de victoire (ex : atteindre la sortie du niveau)
objets.c — états du jeu : menu → jeu → game over / victoire
rendu.c — écrans menu, game over, victoire

✅ Critère : on peut gagner ou perdre, le jeu a un début et une fin.

🟥 Phase 5 — Sprites BMP (Jour 6-7 — 13-14/06)
Objectif : le jeu ressemble à quelque chose.

rendu.c — chargement des BMP avec lisBMPRGB() au démarrage
rendu.c — remplacement de tous les rectangles par les vrais sprites
rendu.c — HUD complet : vies, timer, pièces, nom du niveau
rendu.c — sprite miroir selon direction (flip horizontal)

✅ Critère : le jeu est visuellement identifiable comme un platformer médiéval.

🟩 Phase 6 — Polish & bonus (Jour 8-9 — 15-17/06)
Objectif : se démarquer à la soutenance.

Inertie — le joueur glisse légèrement au lieu de s'arrêter net
Invulnérabilité — frames d'invulnérabilité + clignotement après dégâts
Jump pad — zone qui propulse le joueur très haut
Second niveau — copier/adapter la structure du niveau 1
Parallaxe — arrière-plan qui défile moins vite que le niveau

✅ Critère : le jeu est fun à jouer et impressionne visuellement.

🏁 Phase 7 — Rendu final (Jour 10 — 18/06)
Objectif : zéro stress à 23h00.

make clean → make → vérifier que ça compile from scratch
Vérifier que l'exécutable et les .o ne sont pas dans l'archive
Archive nommée correctement avec le numéro de groupe
Mail aux deux profs avant 23h00
- florian.sananes@yncrea.fr
