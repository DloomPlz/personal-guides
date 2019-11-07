
# Game Methodology

> C# / Unity

## Description

Apres plusieurs tentatives de création de projet auquel j'ai voulu creer un jeu de A à Z sur Unity, je me suis rendu compte qu'il me manquait une methodologie et une approche plus approfondie, et du recul, avant de me lancer dans la programmation, modelisation et texturing.

Je decide donc maintenant de créer un projet de jeu mobile sur Unity, allant de la reflexion du gameplay, de l'architecture de la programmation, etc..

## Sommaire

- [Idée de base](#base)
- [Workflow](#Workflow)
- [Scripts](#scripts)
- [Déploiement](#déploiement)
- [Utilisation](#utilisation)
- [Tests](#tests)
- [Outils utilisés](#outils-utilisés)
- [Architecture](#architecture)
- [Support](#support)
- [Todo](#todo)
- [Auteurs](#auteurs)

## Idée de base

### Aspect du jeu

Le jeu est un Top Down Shooter, type Nazi Zombies Arcade.
Le jeu sera en 3D avec une modélisation en Low-Poly, et sera controlable via des joysticks au bas de l'écran.
Le joueur controlera Lucio via les joysticks.

### L'univers

Le jeu tourne autour d'un petit garcon, Lucio, qui est tourmentée par ces cauchemars. Sa plus grosse peur est une sorte de gros Orc, Drutref Bras de Plomb, qui l'effraie tout le temps.
Afin de sortir des cauchemars, Lucio doit faire face à ces peurs et les combattre. Selon le cauchemar, Lucio aura droit à un arsenal différent.


### Deroulement du jeu

Le jeu est une campagne en solo player sur mobile.
Lucio doit avancer a travers les cauchemars qui sont decoupés en monde, afin de vaincre Drutref Bras de Plomb. Celui-ci laisse a chaque fin de monde, un de ses meilleurs sous fifres qui sert de mini boss.
Il y aura 2 sortes de niveau: de la survie avec un temps imparti, ou zone par zone.

Pour la premiere version du jeu, je voulais une campagne de 5 niveaux.

#### Univers du premier monde
Cette campagne se passera dans un monde qui ressemble a Fossoyeuse, la capitale des morts-vivants dans world of Warcraft. (image fossoyeuse)
Les murs seront en pierre avec des rivieres vertes.
Les ennemies seront des morts-vivants de 3 types: normal, guerrier, mage.
Les zombies normaux seront les ennemies de base, et seront fragile et font peu de degats.
Les zombies guerrier sont proteges par une armure et possederont une epee.
Les zombies mage sont aussi fragile que les normaux mais lancent des projectiles.

Les 5 niveaux de la campagne: 

1er niveau : 

* Introduction de Lucio au joueur, tutoriel des joysticks et bouton de menu. 
* Niveau type survie de zombie normaux.

2e niveau :

* Niveau type zone par zone, introduction du zombie guerrier

3e niveau :

* Introduction du gros boss orc, Drutref Bras de Plomb, qui laissent un sous-fifre comme mini boss pour le 5e niveau.
* Niveau type survie
* Nouveau element: riviere verte faisant des degats au joueur

4 e niveau :

* Nouveau type ennemi : zombie mage
* type zone par zone

5e niveau :

* Mini boss type 1 zone.

#### Fin de monde

A la fin de chaque monde, le joueur recoit une nouvelle arme qu'il pourra utiliser.
Il n'en possede qu'une seule au debut.

#### Score & Monnaie

Lors de la mort d'un ennemi, des pieces sont récoltés automatiquement, servant de score pour les niveaux, et de monnaie.
Les scores sont enregistres a la fin de chaque niveau.

#### Mort du joueur

Le joueur possede 1 vie pour chaque niveau.
Le joueur possede une barre de vie ( ou jauge ). Si celle-ci arrive a 0, le niveau reprendra à 0.

#### Interface utilisateur

Pour la premiere version du jeu, je voudrais un menu principal, oû le joueur verra son score et les différents niveau qu'il a compléter.

En jeu, on aura besoin des deux joysticks, d'un bouton de pause, et du display de score.

### Résumé

Pour résumé, pour la premiere version du jeu:

* 3 types d'ennemies : normal, guerrier, mage
* 2 types de niveau : zone, survie
* Lucio avec une seule arme de base, qui peut se deplacer et tirer
* Des rivieres qui fait des degats au joueur
* Des arrets dans le jeu pour tutoriel, introduction des nouveaux ennemies
* Une interface utilisateur en jeu: 1 bouton pause, 2 joysticks, 1 text score
* Un menu principal gerant les niveaux


## Workflow

Nous avons maitenant nos bases de gameplay, d'univers et d'experience de jeu.
Nous passons donc maintenant au workflow, comment vais-je m'y prendre afin de produire mon idee?
C'est souvent cette étape que l'on oublie ou qu'on délaissé car on ne la trouve pas importante, et on veut au plus vite avoir un apercu de notre idee qui nous plait tant.

En revanche, cette étape est une des plus importantes, car elle nous permettra d'avoir un apercu de l'avancee de notre travail, de ne pas oublier des choses ( ou d'en rajouter), et nous permet de nous organiser en général.

On va commencer par la programmation, on s'occupera que du côté artistique du jeu ( modelisation, texturing, particles effects, etc..) qu'après avoir programmer la totalité de nos scripts.
On utilisera les models 3D deja prédéfinies de Unity, tel que des Box, avant d'avoir de vraies models.

## Workflow

1. Unity
	- Scripts de comportement
	- Scripts d'attaque
	


# Scripts

On programmera tout nos scripts en C#.

On commencera tout d'abord par programmer les comportements du joueur, ainsi que des ennemies. Ceux-ci sont au centre du gameplay de notre jeu, c'est donc une étape cruciale.

Le joueur doit pouvoir:

- Se deplacer
- Tirer
- Avoir de la vie
- Faire des degats
- Avoir une camera qui le suit
- Etre instancié

Les ennemies doivent pouvoir:

- Aller vers le joueur (deplacement)
- Faire des degats au joueur
- Avoir de la vie
- Etre instancié

#### Scripts de comportement

On suppose que lorsque notre niveau commence, aucun GameObject de joueur ou d'ennemie n'est deja présent sur la scene.
Il faudra donc les instancier, et pour cela il nous faut un script de spawn de joueur "PlayerSpawner" et un script de spawn d'ennemie "EnnemySpawner".

Ensuite, pour les scripts de comportement du joueur, il nous faut avant cela les sprites de joysticks.
Chacun des gameobject joysticks auront un script attaché a eux pour le comportement du joueur.

Celui de gauche sera pour le deplacement du joueur "PlayerMovement".
Celui de droite servira a tirer et a faire tourner le joueur vers un certain angle "PlayerRotation".

Ces scripts agiront directement sur le rigidbody du Joueur.

On a donc a ce moment la, un cube qui se deplace grace au joystick gauche, et qui se tourne grace au joystick droit.

Il nous faut aussi un script "PlayerController" afin de reunir toutes les informations du joueur, tels que son rigidbody, son inventaire etc... qui sera pour l'instant vide.

Pour les ennemies, on créera un prefab d'un cube avec un material rouge pour les differencier du joueur.

Pour leur déplacement, on creer un script qui se sert du navmesh intégrer a Unity (chemin le plus court) pour qu'il se dirigie vers le joueur tout en evitant les obstacles. On va appeler ce script "EnnemyMovement".


##### Résumé comportement:

- 2 joysticks / 1 script : PlayerMovement
- 2 scripts d'instanciation de gameObject : PlayerSpawner, EnnemySpawner
- 1 script de mouvement d'ennemi : EnnemyMovement

### Script d'attaque

On rajoute une methode de tir lorsque le joueur bouge le joystick droit.
Pour cela, le joueur necessite une arme (qui sera un petit cube) avec un script "GunController" sur le gameObject.
Ce script servira à tirer, et à faire des degats aux ennemies.

Le script "GunController" sera attaché a toutes les armes que le joueur pourra utiliser, mais pour la premiere version on l'utilisera que sur une seule arme.
Ce script doit pouvoir avoir la plupart de ces variables changeables ( differentes couleur de balles, cadence de tir, etc.. )

Pour les ennemies, il nous faut 2 scripts d'attaque differents "DistanceAttackEnnemy" et "MeleeAttackEnnemy".
Le script d'attaque de distance instanciera des projectiles vers le joueur. Ces projectiles necessitent un script "EnnemyProjectiles", qui servira a blesser le joueur lorsqu'il y aura collision.

Le script de melee se sert du collider des ennemies pour savoir si le joueur est à portée pour l'attaquer.

Pour etre blessé, il faudrait que le joueur et les ennemies aient de la vie.
Pour cela on rajoute 2 scripts: "PlayerHealth" et "EnnemyHealth".
Ils serviront aussi de script de mort, donc de destruction de l'objet.
Periode d'invicibilité lorsque le joueur est touché.

##### Résumé attaque:

- 3 Scripts de lancement d'attaque : GunController, DistanceAttackEnnemy, MeleeAttackEnnemy
- 1 Script de projectiles : EnnemyProjectiles
- 2 scripts de vie : PlayerHealth, EnnemyHealth

#### Script de leveling

On va maintenant gérer les types de niveau, ainsi que leur chargement.
On aura besoin que d'un seul script "LevelController" pour cela.
Ce script prendra en compte le type de level (zone par zone / survie ), le nombre d'ennemies par chambre, tout ce qui sera caracteristique a un niveau.

Pour les niveaux zone par zone, les zones seront instancié dynamiquement et certaines aléatoirement. C'est a dire que seulement 3 salles seront instanciés en meme temps. Des salles seront obligatoires tels que les salles de tutoriel ou de boss.
Il faudra donc un autre script "ZoneLevelController" accrocher a chaque zone pour gerer cela.

### Script de piège/ zones de degats

Petit script de collision "TrapCollision" du joueur avec une zone de danger ou un piège. Le joueur prendra des degats par secondes ou de gros dégats d'un coup.

### Script de pause

Ce script servira a mettre en pause le jeu. Le joueur pourra toujours interagir avec les elements du menu.

### Script de tutoriel

Doit gerer le mouvement du joueur/ de ce qui est présenté etc..
Mise en lumiere ?

### Script de boite de dialogue / description

En accord avec le script d'arret de jeu/ tutoriel, le script de boite de dialogue sera appelé en meme temps si l'on doit display des dialogues.


### Script de score

Manager de score lié au score du joueur.

### Script de menu

Menuing a réviser.

### Idees d'amelioration

- differents types de zones ? bonus zone avec coffre ?
- pieges electriques / sol / pierre qui tombe ?
- zones de spawn au lieu de point precis ?
- Spawn le plus pres du joueur ? 

