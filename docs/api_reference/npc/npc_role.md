# 🎭 Rôle de PNJ (NPC Role)

La classe `Role` est le cœur de la définition du comportement d'un PNJ. Ce n'est pas un composant ECS, mais plutôt un grand objet qui contient toute la configuration, l'état et la logique nécessaires pour qu'un PNJ agisse dans le monde.

Un `Role` est créé par un `BuilderRole`, qui lit une configuration (généralement un fichier JSON) pour initialiser toutes les propriétés du rôle.

## Concepts Clés

### 1. Les "Supports"

Un `Role` délègue une grande partie de sa logique à des classes "support" spécialisées. C'est un bon exemple du principe de responsabilité unique.

*   `CombatSupport`: Gère tout ce qui est lié au combat (attaques, dégâts, etc.).
*   `StateSupport`: Gère l'état du PNJ et les transitions entre les états (par exemple, de "patrouille" à "combat").
*   `WorldSupport`: Gère les interactions du PNJ avec le monde (détection de blocs, etc.).
*   `EntitySupport`: Gère les informations sur l'entité PNJ elle-même (nom, etc.).
*   `MarkedEntitySupport`: Gère les entités "marquées" (par exemple, la cible actuelle du PNJ).
*   `DebugSupport`: Fournit des fonctionnalités pour le débogage.

### 2. Les "Instructions" (Arbre de Comportement)

La logique de décision du PNJ est définie par un arbre de comportement, dont la racine est `rootInstruction`.

*   `rootInstruction`: L'instruction principale qui est exécutée à chaque "tick" du serveur.
*   `interactionInstruction`: Une instruction spéciale qui s'exécute lorsqu'un joueur interagit avec le PNJ.
*   `deathInstruction`: Une instruction qui s'exécute lorsque le PNJ meurt.

La méthode `computeActionsAndSteering()` est appelée à chaque tick et exécute la logique de l'arbre de comportement pour décider quelle action le PNJ doit entreprendre.

### 3. Mouvement (`MotionController` et `Steering`)

Le `Role` gère également le mouvement du PNJ.

*   `MotionController`: Il existe différents contrôleurs de mouvement (par exemple, pour marcher, nager, voler). Le `Role` contient une carte de tous les contrôleurs disponibles et garde une référence vers le contrôleur actif.
*   `Steering`: Des objets qui calculent les forces de direction pour le mouvement (par exemple, pour éviter les obstacles, suivre une cible, ou se déplacer en groupe/flock). Le `Role` a un `bodySteering` (pour le corps) and un `headSteering` (pour la tête).

### 4. Configuration

Un `Role` est presque entièrement défini par la configuration qui est passée à son `BuilderRole`. Voici quelques exemples de propriétés configurables :

*   **Apparence et Inventaire** : `appearance`, `hotbarItems`, `armor`.
*   **Combat** : `initialMaxHealth`, `invulnerable`, `knockbackScale`.
*   **Mouvement** : `inertia`, `collisionRadius`, `flockWeightAlignment`.
*   **Comportement** : L'arbre d'instructions, les interactions, etc.

## Cycle de Vie

Un `Role` a un cycle de vie bien défini, avec des méthodes qui sont appelées à différents moments :

1.  `postRoleBuilt()`: Appelé après que le rôle a été construit.
2.  `loaded()`: Appelé quand le PNJ est chargé en mémoire.
3.  `spawned()`: Appelé quand le PNJ apparaît dans le monde.
4.  `tick()`: Appelé à chaque tick du serveur pour mettre à jour la logique.
5.  `unloaded()`: Appelé quand le PNJ est déchargé.
6.  `removed()`: Appelé quand le PNJ est retiré du monde.

En résumé, la classe `Role` est un conteneur central qui orchestre de nombreux systèmes plus petits pour donner vie à un PNJ. Pour comprendre le comportement d'un PNJ spécifique, il faut analyser le `Role` qui lui est assigné et la configuration qui a été utilisée pour le créer.

## `RoleDebugFlags` (Drapeaux de Débogage)

Le développement de PNJ complexes peut être difficile. Pour aider, le système fournit une série de "drapeaux" de débogage que l'on peut activer (probablement via une commande en jeu) pour visualiser le comportement du PNJ en temps réel.

`RoleDebugFlags` est une `enum` qui contient tous les drapeaux disponibles.

### Types de Drapeaux

Il existe plusieurs catégories de drapeaux :

*   **Traçage du Comportement** :
    *   `TraceFail`: Montre quelles étapes de l'arbre de comportement échouent.
    *   `TraceSuccess`: Montre quelles étapes réussissent.
    *   `TraceSensorFailures`: Idéal pour voir pourquoi une condition n'est pas remplie.

*   **Affichage d'Informations** :
    *   `DisplayState`: Affiche l'état actuel du PNJ (ex: "patrolling").
    *   `DisplayHP`, `DisplayStamina`: Affiche la vie et l'endurance.
    *   `DisplayTarget`: Affiche la cible actuelle.
    *   `DisplayName`, `DisplayInternalId`: Affiche le nom du rôle et l'ID interne de l'entité.

*   **Visualisation du Mouvement** :
    *   `MotionControllerSteer`: Affiche les informations de direction du contrôleur de mouvement.
    *   `Collisions`, `BlockCollisions`: Montre les détections de collision.
    *   `VisAvoidance`, `VisSeparation`: Dessine des vecteurs dans le monde pour montrer comment le PNJ essaie d'éviter les obstacles ou les autres entités.
    *   `Pathfinder`: Affiche l'état du pathfinding (recherche de chemin).

### Presets

Pour ne pas avoir à activer les drapeaux un par un, il existe des "presets" (pré-réglages) qui regroupent des drapeaux communs.

*   `none`: Désactive tous les drapeaux.
*   `all`: Active tous les drapeaux (attention, peut être très verbeux).
*   `move`: Affiche les informations de base sur le mouvement et les collisions.
*   `steer`: Similaire à `move` mais avec plus de détails sur la direction.
*   `display`: Active la plupart des affichages d'informations.

Ces outils sont indispensables pour comprendre pourquoi un PNJ ne se comporte pas comme prévu, et pour ajuster finement sa configuration.
