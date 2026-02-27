# InstaKill - Système 3 Profils

## Vue d'ensemble

Le système InstaKill permet d'exécuter des combos d'attaque instantanée (insta-kill) en une seule touche.  
Il est organisé en **3 profils indépendants**, chacun avec sa propre touche, son type d'attaque et ses options.

**Aucun comportement automatique** — tout est déclenché manuellement par l'utilisateur.

---

## Profils

| Profil | Touche par défaut | Type par défaut | Description |
|--------|-------------------|-----------------|-------------|
| **1**  | `Y`               | Normal          | Profil principal, actif par défaut |
| **2**  | *Aucune*          | Reverse         | Désactivé par défaut (touche = None) |
| **3**  | *Aucune*          | Spike Tick      | Désactivé par défaut (touche = None) |

> Pour activer un profil désactivé, assignez-lui une touche dans le menu InstaKill.  
> Pour désactiver un profil, appuyez sur `Suppr` lors de l'assignation de la touche.

---

## Paramètres par profil

Chaque profil possède 5 paramètres :

### Key (Touche)
- **Type** : Bouton hotkey (`hotkeyInput`)
- **Effet** : Touche clavier qui déclenche le combo insta
- **Valeur spéciale** : `None` = profil désactivé (aucune touche assignée)

### Type
- **Type** : Bouton cyclique (`option-button`)
- **Effet** : Détermine quel combo d'attaque est exécuté
- **Valeurs possibles** (cliquer pour cycler) :

| Type | ID interne | Description |
|------|-----------|-------------|
| **Normal** | `normal` | Hat Bull → Primary hit → Secondary hit → Reset |
| **Reverse** | `reverse` | Secondary hit → Primary hit → Reset (commence par l'arme secondaire) |
| **No Bull** | `noBull` | Musket gear → Primary → Secondary → Reset (sans Bull Hat) |
| **Spike Tick** | `spikeTick` | Bull hat → Primary → Musket gear → Continue hit → Reset |
| **Counter** | `counter` | Secondary + Shadow Wings → Primary + Bull → Reset |
| **One Tick** | `oneTick` | Identique à Counter (Shadow Wings → Bull combo) |
| **Range Sync** | `rangeSync` | Musket/Flipper → Crossbow shot → Reset (attaque à distance) |
| **Sync Hit** | `syncHit` | Musket gear equip → Bull + Flipper attack → Reset |
| **Boost Tick** | `boostTick` | Boost pad → Spike behind + Secondary → Bull + Primary → Reset |

### Auto-aim
- **Type** : Checkbox (on/off)
- **Par défaut** : Activé (`true`)
- **Activé** : Le combo vise automatiquement l'ennemi le plus proche
- **Désactivé** : Le combo utilise la direction de la souris du joueur
- **Si activé et aucun ennemi à proximité** : Le combo est annulé immédiatement

### Check can insta
- **Type** : Checkbox (on/off)
- **Par défaut** : Activé (`true`)
- **Activé** : Le combo ne se lance que si le dégât calculé est ≥ 100 HP (kill garanti)
- **Désactivé** : Le combo se lance quoi qu'il arrive
- **Calcul** : Prend en compte les armes équipées, les hats (Bull, Musket, etc.) et le hat de l'ennemi

### Delay (Délai en ticks)
- **Type** : Slider (0 à 5)
- **Par défaut** : `0`
- **Effet** : Ajoute un délai entre chaque étape du combo
  - `0` = exécution la plus rapide possible (1 tick par étape)
  - `1` = 1 tick d'attente entre chaque étape
  - `5` = 5 ticks d'attente entre chaque étape
- **Usage** : Peut aider à synchroniser le timing dans certaines situations réseau

---

## Fonctionnement technique

### Déclenchement (handleKeydown)
```
Appui touche → Boucle sur profils 1-3
  → Si touche correspond ET spikeTick pas déjà actif :
    → Si checkCan activé : vérifier canInsta (sinon ignorer)
    → Lancer startInsta(type, { autoAim, delay })
    → Sortir de la boucle
```

### Exécution (SpikeTick.postTick)
```
Chaque tick serveur (~100ms) :
  → Si pas actif : ne rien faire
  → Si delay > 0 : accumuler les ticks, skip si pas encore le moment
  → Dispatcher vers la méthode execute correspondant au type
  → Chaque méthode gère ses propres ticks (equip hat, attack, switch, reset)
```

### Angle d'attaque (getAngle)
```
Si autoAim désactivé → utiliser mouse.angle (direction souris)
Si autoAim activé :
  → Si ennemi présent → angle vers l'ennemi le plus proche
  → Sinon → fallback sur mouse.angle
```

---

## Settings internes

| Setting | Type | Défaut P1 | Défaut P2 | Défaut P3 |
|---------|------|-----------|-----------|-----------|
| `instaKey1/2/3` | string \| null | `"KeyY"` | `null` | `null` |
| `instaType1/2/3` | string | `"normal"` | `"reverse"` | `"spikeTick"` |
| `instaAutoAim1/2/3` | boolean | `true` | `true` | `true` |
| `instaCheckCan1/2/3` | boolean | `true` | `true` | `true` |
| `instaDelay1/2/3` | number (0-5) | `0` | `0` | `0` |

---

## Intégrations avec d'autres modules

- **Autohat** : Quand un insta est actif (`spikeTick.active`), l'autohat ne change pas de chapeau
- **getAttackAngle** : Quand un insta est actif (`spikeTick.isActive`), l'angle d'attaque est forcé sur `useAngle`
- **getBestUtilityHat** : Lit `spikeTick.isActive` et `spikeTick.tickAction` pour adapter le hat
- **CheckCanInsta** : Module indépendant qui calcule `canInsta` à chaque tick — utilisé par les profils avec `checkCan` activé

*Documentation créer pour Glotus Client Multibox Edition v2.2+*
