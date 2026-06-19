## PARTIE 1 — Présentation du jeu (~40 secondes)
**🎤 Paul**

### Slide 1 — Titre

**À dire :**
> « Bonjour, on est le groupe [X]. On a développé **Medieval Fantasy**, un jeu de plateformes en C avec la bibliothèque GfxLib.  
> Le joueur incarne un chevalier qui traverse 9 niveaux dans un univers médiéval — des plaines au lever du soleil jusqu'aux donjons souterrains. »

### Slide 2 — Aurélie Screenshots du jeu (3-4 captures)
> Contenu slide : menu principal, gameplay extérieur, donjon, écran de victoire

**À dire :**
> « Voici le menu, la sélection de niveau avec les records, un niveau extérieur avec parallaxe, et un donjon avec pièges et ennemis.  
> On a implémenté toutes les fonctionnalités demandées dans le sujet et plusieurs bonus. Le projet est découpé en 7 modules — on va vous présenter qui a fait quoi. »

### Slide 3 Luna — Architecture & Fonctionnalités   
**À dire :**


## PARTIE 2 — Contributions individuelles (~25 s chacun)

> [!IMPORTANT]
> Chaque personne : **2-3 phrases max**, pas de détails de code.  
> Formule : "j'ai fait X, ça fonctionne en faisant Y, le point clé c'est Z."

---

### 🎤 Paul Gomez — Objets & Score (~25 s)
> Slide : screenshot du HUD (pièces, vies) + zone de sortie

**À dire :**
> « J'ai développé le module **objets** : la collecte des pièces par détection de collision bounding box, le bonus de vie toutes les 10 pièces, et la **condition de victoire** quand le joueur atteint la zone de sortie.  
> J'ai aussi écrit la fonction `initialiserJeu` qui remet tout l'état du jeu à zéro à chaque lancement de niveau. »

**→ Passer la parole à Luna**

---

### 🎤 Luna Tardiveau — Menus & Sauvegarde (~25 s)
> Slide : screenshot du menu principal + sélection de niveau avec records affichés

**À dire :**
> « Je me suis occupée des **menus** et de la **sauvegarde des records**. J'ai créé le menu principal, l'écran de sélection des 9 niveaux avec navigation au clavier, l'écran d'aide, et les écrans de victoire et de game over.  
> Les meilleurs temps sont sauvegardés dans un fichier texte avec `fprintf` et `fscanf`, et rechargés automatiquement au lancement. »

**→ Passer la parole à Aurélie**

---

### 🎤 Aurélie Chodron de Courcel — Ennemis (~25 s)
> Slide : screenshot d'un ennemi en charge + schéma simplifié (patrouille → détection → charge)

**À dire :**
> « J'ai codé le module **ennemi** : le déplacement en patrouille avec demi-tour automatique aux bords et aux murs, et l'**IA de charge** qui s'active quand le joueur entre dans le rayon de détection.  
> Il y a 3 types d'ennemis — le slime vert, le slime rouge plus rapide, et le démon — chacun avec des vitesses et portées de détection différentes. »

**→ Passer la parole à Mathieu**

---

### 🎤 Mathieu Cortes — Niveaux (~25 s)
> Slide : extrait d'un blueprint ASCII côte-à-côte avec le screenshot du niveau correspondant

**À dire :**
> « J'ai travaillé sur le système de **blueprints** : chaque niveau est défini comme une matrice de caractères ASCII. Un parseur lit cette grille et génère automatiquement les plateformes, les ennemis, les pièces et le point de spawn.  
> J'ai aussi implémenté la **caméra** avec suivi horizontal fluide et dead-zone verticale pour que le joueur reste toujours visible. »

**→ Retour à Mael**

---

## PARTIE 3 — Mael : Moteur, Rendu & Retour technique (~1 min 30)
**🎤 Mael**

### Slide — Ma contribution : moteur physique & rendu (~30 s)

**À dire :**
> « De mon côté, j'ai conçu les **structures de données** du jeu — la structure `JeuEtat` qui centralise tout l'état — et le **moteur physique** dans `joueur.c` : gravité, collisions AABB avec toutes les plateformes, et les mécaniques avancées comme le **coyote time**, le **jump buffer**, le **double saut** et le **dash**.  
> J'ai aussi développé tout le module de **rendu** : chargement des BMP, système d'animation multi-frames, affichage des plateformes en 3 parties, parallaxe multicouche, et le HUD.


### Slide — Mael - Problème 1 : Lecture continue des touches (~20 s)

> Contenu slide : "GfxLib = événements ponctuels" → "XQueryKeymap = état continu"

**À dire :**
> « Premier problème majeur : **GfxLib ne signale les touches qu'à l'appui**, pas tant qu'elles sont maintenues. Pour un platformer, c'est bloquant — on a besoin de savoir en continu si la flèche droite est enfoncée.  
> On a contourné ça en utilisant directement **XQueryKeymap**, une fonction de la bibliothèque X11, qui retourne l'état complet du clavier à chaque frame. »

### Slide - Mathieu Problème 2 : Transparence des sprites (~20 s)

> Contenu slide : "BMP 24 bits = pas d'alpha" → "conversion ARVB + magenta = transparence"

**À dire :**
> « Deuxième problème : GfxLib ne gère que les BMP 24 bits, **sans canal alpha**. On a écrit notre propre fonction de chargement qui convertit chaque pixel en format ARVB 32 bits. Tous les pixels de couleur **magenta pur** (255, 0, 255) reçoivent un alpha à zéro, ce qui les rend transparents. »

### Slide finale Mael — Conclusion (~10 s)

> Contenu slide : 2-3 pistes d'évolution (boss, power-ups, éditeur de niveaux)

**À dire :**
> « Si on avait eu plus de temps, on aurait aimé ajouter un boss de fin de jeu et un éditeur de niveaux intégré. Merci pour votre attention, on est prêts pour vos questions. »

---

---



