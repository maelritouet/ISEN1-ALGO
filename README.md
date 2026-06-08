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

| Date | Étape |
|---|---|
| 08/06 | Lancement — définition structs & archi |
| 10/06 | Point d'avancement |
| 12/06 | Point d'avancement |
| 15/06 | Point d'avancement |
| 17/06 | Point d'avancement |
| **18/06 23h00** | **Rendu final** |
| 19/06 | Soutenances |

---

## 📬 Rendu

Archive à envoyer à :
- lucas.giraud@yncrea.fr
- florian.sananes@yncrea.fr
