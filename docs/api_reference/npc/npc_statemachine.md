# 🔄 Machine à États de PNJ (NPC State Machine)

La machine à états est un concept fondamental qui dicte le comportement d'un PNJ. Un PNJ peut être dans un état à la fois (par exemple, `Idle`, `Patrolling`, `InCombat`), et des "transitions" définissent comment et quand le PNJ passe d'un état à un autre.

Ce document décrit les "builders" qui permettent de configurer cette machine à états.

## `BuilderStateTransition`

Ce builder est le bloc de construction principal pour définir ce qui se passe *pendant* une transition d'état.

*   **Objectif** : Associer une liste d'actions à une ou plusieurs transitions d'état. Par exemple, "quand le PNJ passe de l'état `Patrolling` à `InCombat`, jouer un son de cri et dégainer son épée".

### Configuration (JSON)

Un objet `StateTransition` est défini par les propriétés suivantes :

*   `"States"` (tableau, requis) : Définit les transitions concernées. C'est un tableau d'objets "edges" (arêtes).
    *   Chaque objet "edge" a une propriété `"From"` et/ou `"To"`, qui peuvent être un nom d'état unique ou un tableau de noms d'états.
    *   Si `"From"` est omis, la transition s'applique depuis n'importe quel état.
    *   Si `"To"` est omis, la transition s'applique vers n'importe quel état.
*   `"Actions"` (objet, requis) : Une `ActionList` (liste d'actions) à exécuter lorsque la transition se produit.
*   `"Enabled"` (booléen, optionnel, défaut: `true`): Permet de désactiver cette définition de transition.

### Exemple d'utilisation (conceptuel)

Dans le fichier de configuration de la machine à états d'un PNJ :

```json
{
  "transitions": [
    {
      // Transition 1: Quand on entre en combat depuis n'importe quel état
      "States": [
        { "To": "InCombat" }
      ],
      "Actions": {
        "actions": [
          { "action": "PlaySound", "Sound": "npc.orc.aggro" },
          { "action": "EquipItem", "Item": "orc_sword" }
        ]
      }
    },
    {
      // Transition 2: Quand on passe de InCombat à Patrolling
      "States": [
        { "From": "InCombat", "To": "Patrolling" }
      ],
      "Actions": {
        "actions": [
          { "action": "SheatheItem" }
        ]
      }
    }
  ]
}
```

Ce builder crée un objet `StateTransition` qui est ensuite utilisé par le `StateTransitionController` pour gérer le comportement du PNJ.

## `BuilderStateTransitionController`

Ce builder est le conteneur de haut niveau pour toutes les transitions d'état d'un PNJ. Il lit simplement une liste de définitions de `StateTransition` et crée un `StateTransitionController`.

*   **Objectif** : Construire l'objet `StateTransitionController` qui sera attaché au `Role` du PNJ pour gérer toutes ses transitions d'état.

### Configuration (JSON)

Ce builder attend un tableau (une liste) d'objets `StateTransition`. Le nom de la propriété n'est pas fixe, car le builder lit directement le tableau. Souvent, dans la configuration globale du PNJ, cela ressemblera à :

```json
{
  // ... autres propriétés du PNJ
  "stateTransitions": [
    {
      "States": [ { "To": "InCombat" } ],
      "Actions": { /* ... */ }
    },
    {
      "States": [ { "From": "InCombat", "To": "Patrolling" } ],
      "Actions": { /* ... */ }
    }
  ]
}
```

Le `BuilderStateTransitionController` serait responsable de lire le contenu du tableau `stateTransitions`.

Le `StateTransitionController` ainsi créé est l'objet qui, à chaque changement d'état, va consulter sa liste de `StateTransition` pour voir si des actions doivent être exécutées.

## `BuilderStateTransitionEdges`

Cette classe est un helper utilisé à l'intérieur de `BuilderStateTransition` pour définir les "arêtes" (les chemins) d'une transition.

*   **Objectif** : Spécifier un ensemble d'états de départ (`From`) et d'états d'arrivée (`To`) pour une transition.

### Configuration (JSON)

Un objet "edge" est défini par les propriétés suivantes :

*   `"From"` (string ou tableau de strings, optionnel) : Le ou les états de départ.
*   `"To"` (string ou tableau de strings, optionnel) : Le ou les états d'arrivée.
*   `"Priority"` (entier, optionnel, défaut: 0) : La priorité des actions de cette transition.
*   `"Enabled"` (booléen, optionnel, défaut: `true`): Permet de désactiver cette arête.

**Règles importantes** :
*   Un état ne peut pas transitionner vers lui-même dans la même arête.
*   Si `"From"` n'est pas défini, cela signifie "depuis n'importe quel état".
*   Si `"To"` n'est pas défini, cela signifie "vers n'importe quel état".

### Logique Interne

Ce builder prend les noms des états (ex: `"InCombat"`) et les convertit en "indices" (des entiers) pour des comparaisons plus rapides au moment de l'exécution.
