# Documentation - Glotus Client MooMoo.io (Multibox Edition)

## Table des matières

1. [Vue d'ensemble](#vue-densemble)
2. [Onglet Keybinds](#onglet-keybinds)
3. [Onglet Combat](#onglet-combat)
4. [Onglet Visuals](#onglet-visuals)
5. [Onglet Misc](#onglet-misc)
6. [Onglet Devtool](#onglet-devtool)
7. [Onglet Multibox](#onglet-multibox)
8. [Onglet InstaKill](#onglet-instakill)
9. [Système de modules](#système-de-modules)

---

## Vue d'ensemble

Le script Glotus Client est un client modifié pour MooMoo.io offrant des fonctionnalités avancées de combat, de placement automatique, de gestion des chapeaux et d'instakill. Le menu s'ouvre avec la touche `Escape` par défaut et contient 7 onglets configurables.

### Architecture

Le script utilise un **système de modules** en pipeline : chaque module possède une méthode `postTick()` appelée à chaque tick du jeu (~111ms). L'ordre d'exécution est important car certains modules dépendent des résultats d'autres modules.

Les paramètres sont sauvegardés via des cookies et sont automatiquement restaurés au rechargement.

---

## Onglet Keybinds

Configuration des touches de contrôle du jeu.


| Paramètre             | Description                                                     |
| ------------------------ | ----------------------------------------------------------------- |
| **Primary**            | Touche pour sélectionner l'arme principale (défaut :`Digit1`) |
| **Secondary**          | Touche pour sélectionner l'arme secondaire (défaut :`Digit2`) |
| **Food**               | Touche pour placer de la nourriture (défaut :`KeyQ`)           |
| **Wall**               | Touche pour placer un mur (défaut :`KeyF`)                     |
| **Spike**              | Touche pour placer un spike (défaut :`KeyV`)                   |
| **Windmill**           | Touche pour placer un moulin (défaut :`KeyN`)                  |
| **Trap**               | Touche pour placer un piège (défaut :`KeyT`)                  |
| **Turret**             | Touche pour placer une tourelle (défaut :`KeyH`)               |
| **Farm**               | Touche pour placer une ferme (défaut :`KeyG`)                  |
| **Spawn**              | Touche pour placer un point de spawn (défaut :`KeyJ`)          |
| **Up/Down/Left/Right** | Touches de déplacement (défaut : WASD)                        |
| **Autoattack**         | Active/désactive l'auto-attaque (défaut :`KeyE`)              |
| **Lock rotation**      | Verrouille la rotation de la caméra (défaut :`KeyX`)          |
| **Toggle menu**        | Ouvre/ferme le menu (défaut :`Escape`)                         |
| **Toggle chat**        | Ouvre le chat (défaut :`Enter`)                                |
| **Toggle shop**        | Ouvre la boutique (défaut :`KeyP`)                             |
| **Toggle clan**        | Ouvre le menu de clan                                           |
| **Boost spike key**    | Active le boost spike combo (défaut :`KeyR`)                   |
| **Spike ring key**     | Place un anneau de spikes autour du joueur (défaut :`KeyN`)    |
| **Turret ring key**    | Place un anneau de tourelles autour du joueur                   |

---

## Onglet Combat

### Section Autohat

Gestion automatique des chapeaux et accessoires.


| Paramètre               | Description                                                                                            |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Biome hats**           | Équipe automatiquement le chapeau adapté au biome (Winter Cap pour la neige, Flipper Hat pour l'eau) |
| **Autoemp**              | Équipe automatiquement le casque EMP (id 22) quand les tourelles ennemies sont proches                |
| **Anti enemy**           | Active la détection anti-ennemi — adapte le chapeau défensif quand un ennemi est à portée         |
| **Anti animal**          | Équipe un chapeau défensif quand un animal dangereux est proche                                      |
| **Anti spike**           | Active la protection automatique contre les spikes ennemis                                             |
| **Tail remove distance** | Distance à laquelle l'accessoire de queue est retiré (slider 50-600)                                 |
| **Tail: near enemy**     | Retire l'accessoire de queue quand un ennemi est proche                                                |
| **Tail: moving**         | Retire l'accessoire de queue pendant le déplacement                                                   |
| **Tail: on hit**         | Retire l'accessoire de queue quand le joueur est touché                                               |

### Section Healing

Gestion automatique des soins.


| Paramètre         | Description                                                         |
| -------------------- | --------------------------------------------------------------------- |
| **Autoheal**       | Active le soin automatique quand la vie descend sous le seuil       |
| **Healing speed**  | Vitesse de soin (slider 0-50, en millisecondes)                     |
| **Heal threshold** | Seuil de vie en dessous duquel le soin se déclenche (slider 1-99%) |

### Section Placement

Placement automatique de structures.


| Paramètre     | Description                                                                                                                                               |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Autoplacer** | Place automatiquement des spikes et pièges autour des ennemis piégés. Analyse les meilleures positions de placement en fonction de l'angle de l'ennemi |
| **Automill**   | Place automatiquement des moulins derrière le joueur en mode sandbox                                                                                     |

### Section Defense


| Paramètre            | Description                                                                                                                                                                      |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Autobreak**         | Casse automatiquement les pièges ennemis quand le joueur est piégé. Oriente l'arme vers le piège le plus proche                                                              |
| **Anti projectile**   | Détecte les projectiles ennemis entrants. Équipe le casque EMP (id 22) pour les tirs multiples ou le casque Soldier (id 6) pour les tirs simples. Déclenche un soin d'urgence |
| **Anti sync healing** | Effectue un cycle de soins rapides (3 soins à 75ms d'intervalle) quand une menace est détectée. Protège contre les combos d'instakill synchronisés                          |

### Section Advanced


| Paramètre           | Description                                                                                                                                                                     |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Auto push**        | Pousse automatiquement les ennemis piégés vers les spikes alliés. Analyse la position des spikes proches du piège et ajuste la direction du joueur                          |
| **Auto replace**     | Reconstruit automatiquement les structures détruites. Surveille les objets possédés par le joueur et replace une structure identique au même angle quand elle est détruite |
| **Place visible**    | Affiche un aperçu visuel des placements récents. Chaque placement affiche un cercle vert semi-transparent qui disparaît après 300ms                                         |
| **Hat changer mode** | Mode de changement de chapeau :**static** (comportement par défaut) ou **dynamic** (changement contextuel basé sur la situation)                                              |

#### Mode Hat Changer Dynamic

En mode dynamique, le système de chapeau analyse la situation en temps réel :

- **Ennemi proche avec Bull Helmet** → Équipe le Spike Gear (id 11) pour renvoyer les dégâts
- **Ennemi proche et attaque possible** → Équipe le Bull Helmet (id 7) pour +50% de dégâts
- **Ennemi proche sans possibilité d'attaque** → Équipe le Soldier Helmet (id 6) pour -25% de dégâts reçus
- **Zone de neige (y < 2400)** → Équipe le Winter Cap (id 15)
- **Zone d'eau (6850 < y < 7550)** → Équipe le Flipper Hat (id 31)
- **Accessoire dynamique** : Corrupt X Wings (id 21) en attaque, Shadow Wings (id 19) en défense, Angel Wings (id 13) par défaut

---

## Onglet Visuals

### Section Tracers

Lignes visuelles pointant vers les entités.


| Paramètre       | Description                                             |
| ------------------ | --------------------------------------------------------- |
| **Enemies**      | Tracers vers les ennemis (couleur personnalisable)      |
| **Teammates**    | Tracers vers les coéquipiers (couleur personnalisable) |
| **Animal**       | Tracers vers les animaux (couleur personnalisable)      |
| **Notification** | Tracers de notification (couleur personnalisable)       |
| **Arrows**       | Flèches directionnelles                                |

### Section Markers

Marqueurs visuels sur les entités.


| Paramètre       | Description                                              |
| ------------------ | ---------------------------------------------------------- |
| **Item Markers** | Marqueurs sur les objets (couleur personnalisable)       |
| **Teammates**    | Marqueurs sur les coéquipiers (couleur personnalisable) |
| **Enemies**      | Marqueurs sur les ennemis (couleur personnalisable)      |

### Section Player

Informations visuelles sur le joueur.


| Paramètre            | Description                                                              |
| ----------------------- | -------------------------------------------------------------------------- |
| **Weapon XP Bar**     | Barre d'expérience de l'arme                                            |
| **Turret Reload Bar** | Barre de rechargement de la tourelle du joueur (couleur personnalisable) |
| **Weapon Reload Bar** | Barre de rechargement de l'arme (couleur personnalisable)                |
| **Render HP**         | Affiche les points de vie numériquement                                 |

### Section Object

Informations visuelles sur les objets.


| Paramètre            | Description                                                            |
| ----------------------- | ------------------------------------------------------------------------ |
| **Turret Reload Bar** | Barre de rechargement des tourelles placées (couleur personnalisable) |
| **Item Health Bar**   | Barre de vie des structures destructibles (couleur personnalisable)    |

### Section Other


| Paramètre            | Description                                        |
| ----------------------- | ---------------------------------------------------- |
| **Item counter**      | Compteur d'objets placés                          |
| **Render grid**       | Affiche la grille de la carte                      |
| **Windmill rotation** | Active/désactive la rotation visuelle des moulins |
| **Entity Danger**     | Affiche le niveau de danger des entités           |

---

## Onglet Misc

### Section Other


| Paramètre     | Description                                     |
| ---------------- | ------------------------------------------------- |
| **Autospawn**  | Réapparaît automatiquement à la mort         |
| **Autoaccept** | Accepte automatiquement les invitations de clan |

### Section Menu


| Paramètre       | Description                    |
| ------------------ | -------------------------------- |
| **Transparency** | Active la transparence du menu |

---

## Onglet Devtool

Outils de développement et de débogage (hitbox, collision, etc.).

---

## Onglet Multibox

Système de contrôle de plusieurs bots simultanément.


| Paramètre          | Description                 |
| --------------------- | ----------------------------- |
| **Enable Multibox** | Active le système multibox |
| **Bot count**       | Nombre de bots à créer    |
| **Bot prefix**      | Préfixe du nom des bots    |

Les bots suivent le joueur principal, synchronisent leurs améliorations, et peuvent effectuer des actions automatiques (placement, attaque).

---

## Onglet InstaKill

Configuration complète du système d'instakill — combos d'attaque destinés à éliminer un ennemi en un minimum de ticks.

### Section General


| Paramètre          | Description                                                                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Check can insta** | Vérifie avant chaque tentative si l'instakill est mathématiquement possible. Calcule les dégâts combinés (arme primaire × Bull + secondaire + Musket turret) et réduit selon le chapeau ennemi |
| **Insta key**       | Touche pour déclencher manuellement un instakill (défaut :`KeyY`)                                                                                                                                   |

### Section Auto InstaKill Types

Types d'instakill automatiques — se déclenchent quand un ennemi est à portée (~250 unités).


| Type                  | Séquence                                                                                                      | Description                                                                                                                                              |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Normal instakill**  | Tick 0 : Primaire + Bull(7) + Bloodthirsty(21) → Tick 1 : Secondaire + Musket(53)/Soldier(6) → Tick 3 : Stop | Combo standard le plus fiable. Envoie l'arme primaire avec Bull pour +50% dégâts, puis l'arme secondaire avec le hat turret pour dégâts additionnels |
| **Reverse instakill** | Tick 0 : Secondaire + Musket(53) + Bloodthirsty(21) → Tick 1 : Primaire + Bull(7) → Tick 3 : Stop            | Inverse de normal — commence par le secondaire pour surprendre l'ennemi avec un projectile, suivi d'un coup de mêlée avec Bull                        |
| **No bull instakill** | Tick 0 : Primaire + Musket(53) + Bloodthirsty(21) → Tick 1 : Secondaire → Tick 3 : Stop                      | N'utilise pas le Bull Helmet — utile contre les ennemis portant le Spike Gear (id 11) qui renvoie les dégâts                                          |
| **Spike tick insta**  | Tick 0 : Primaire + Bull(7) + Bloodthirsty(21) → Tick 1 : Musket(53) → Tick 4 : Stop + Soldier(6)            | Combo étendu sur plus de ticks, utilise le hat tourelle pour des dégâts supplémentaires à distance                                                  |
| **Counter insta**     | Tick 0 : Secondaire + Musket(53) + Shadow(19) → Tick 1 : Primaire + Bull(7) → Tick 2 : Stop                  | Combo de contre-attaque rapide — utilise Shadow Wings pour la vitesse de déplacement supplémentaire                                                   |

### Section Manual InstaKill Types

Types d'instakill manuels — déclenchés uniquement par leurs touches respectives.


| Type                 | Touche                            | Séquence                                                                                                                      | Description                                                                                                            |
| ---------------------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **One tick insta**   | `oneTicKey` (défaut : `KeyT`)    | Tick 0 : Secondaire + Musket(53) + Shadow(19) → Tick 1 : Primaire + Bull(7) → Tick 2 : Stop                                  | Similaire au counter mais déclenché manuellement pour un timing précis                                              |
| **Range sync**       | `rangeSyncKey` (défaut : `KeyX`) | Tick 0 : Secondaire + Musket(53)/Samurai(20) + Monkey Tail(11) → Tick 1 : Stop                                                | Attaque à distance synchronisée — tire le projectile avec boost de vitesse puis s'arrête immédiatement            |
| **Sync hit**         | `syncHitKey` (défaut : `KeyB`)   | Tick 0 : Primaire + Musket(53) → Tick 1 : Bull(7) + Bloodthirsty(21) + attaque → Tick 2 : Stop                               | Synchronise l'équipement du hat avec l'attaque pour maximiser les dégâts au moment de l'impact                      |
| **Boost tick insta** | Activé dans le menu              | Tick 0 : Booster(12) + Shadow(19) → Tick 1 : Secondaire + Musket(53) + piège → Tick 2 : Primaire + Bull(7) → Tick 3 : Stop | Combo avancé qui utilise le boost de vitesse pour s'approcher rapidement, place un piège, puis enchaine les dégâts |

### Logique de vérification (Check Can Insta)

La formule de calcul des dégâts potentiels :

$$
\text{Total} = (\text{Primaire}_{dmg} \times 1.5) + \text{Secondaire}_{dmg} + 25_{turret}

$$

Réduction selon le chapeau ennemi :

- Soldier Helmet (id 6) : Total × 0.75
- EMP Helmet (id 22) : Total × 0.75

L'instakill est possible si $\text{Total} \geq 100$ (points de vie max d'un joueur).

---

## Système de modules

### Ordre d'exécution du pipeline

```
1.  AutoAccept     → Accepte les invitations de clan
2.  AntiInsta      → Soins d'urgence contre les instakills
3.  ShameReset     → Gestion du shame counter (anti-spam heal)
4.  AutoPlacer     → Placement auto de spikes/pièges
5.  Placer         → Gestion des placements manuels
6.  AutoMill       → Placement auto de moulins
7.  PlacementExec  → Exécution de la file d'attente de placements
8.  Reloading      → Suivi des temps de rechargement
9.  AutoPush       → Pousse les ennemis vers les spikes
10. AutoReplace    → Remplace les structures détruites
11. AntiProj       → Détection des projectiles entrants
12. AntiSyncHeal   → Soins rapides anti-sync
13. CheckCanInsta  → Vérification de faisabilité d'instakill
14. SpikeTick      → Exécution des combos d'instakill
15. AutoBreak      → Cassage auto des pièges
16. BoostSpike     → Combo boost + spike
17. PreAttack      → Pré-calcul de l'attaque
18. AutoHat        → Gestion auto des chapeaux
19. PlaceVisible   → Rendu visuel des placements
20. UpdateAttack   → Envoi de l'attaque au serveur
21. UpdateAngle    → Envoi de l'angle au serveur
```

### Chapeaux importants (référence rapide)


| ID | Nom            | Effet principal                               |
| ---- | ---------------- | ----------------------------------------------- |
| 6  | Soldier Helmet | -25% dégâts reçus, -6% vitesse             |
| 7  | Bull Helmet    | +50% dégâts infligés, -5 HP/s, -4% vitesse |
| 11 | Spike Gear     | Renvoie 45% des dégâts reçus               |
| 12 | Booster Hat    | +16% vitesse                                  |
| 15 | Winter Cap     | Vitesse normale dans la neige                 |
| 20 | Samurai Armor  | -22% temps d'attaque                          |
| 22 | EMP Helmet     | Immunité aux tourelles, -30% vitesse         |
| 31 | Flipper Hat    | Contrôle dans l'eau                          |
| 40 | Tank Gear      | ×3.3 dégâts aux bâtiments, -70% vitesse   |
| 53 | Turret Gear    | Tourelle mobile, -30% vitesse                 |
| 55 | Bloodthirster  | +20% dégâts, soins sur dégâts infligés   |

### Accessoires importants


| ID | Nom             | Effet principal                 |
| ---- | ----------------- | --------------------------------- |
| 11 | Monkey Tail     | +35% vitesse, -80% dégâts     |
| 13 | Angel Wings     | +3 HP/s régénération         |
| 19 | Shadow Wings    | +10% vitesse                    |
| 21 | Corrupt X Wings | Renvoie 25% des dégâts reçus |

---


*Documentation créer pour Glotus Client Multibox Edition v2.0+*
