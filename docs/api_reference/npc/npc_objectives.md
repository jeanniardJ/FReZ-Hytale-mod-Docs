# 🤖 Objectifs de PNJ (NPC Objectives)

Le système d'objectifs de PNJ permet de créer des comportements complexes pour les personnages non-joueurs, comme l'accomplissement de tâches.

## Builders d'Actions

Les "builders" sont des classes qui lisent une configuration (souvent depuis un fichier JSON) et créent une instance d'une action ou d'un autre objet.

### `BuilderActionCompleteTask`

Ce builder est utilisé pour créer une action qui permet à un PNJ de compléter une tâche.

*   **Héritage** : `BuilderActionPlayAnimation`
    *   Cela signifie qu'il peut, en plus de sa logique principale, jouer une animation.
*   **Objectif** : Dire au PNJ de terminer une tâche. Les tâches disponibles sont déterminées par un autre composant, le `SensorCanInteract`.
*   **Type d'instruction requis** : `Interaction`. Cette action ne peut être utilisée que dans un contexte d'interaction.

#### Configuration (JSON)

Lors de la définition du comportement d'un PNJ, vous pouvez utiliser cet objet.

*   `"PlayAnimation"` (booléen, optionnel, défaut: `true`): Si `true`, le PNJ jouera l'animation associée à la tâche qu'il complète.

#### Exemple d'utilisation (conceptuel)

Dans un fichier de configuration de PNJ au format JSON :

```json
{
  "onInteract": {
    "action": "CompleteTask",
    "PlayAnimation": false // Le PNJ ne jouera pas d'animation
  }
}
```

Ce builder crée une `ActionCompleteTask`, qui sera ensuite exécutée par le système de comportement du PNJ.

### `BuilderActionStartObjective`

Ce builder crée une action qui démarre un objectif pour le joueur avec lequel le PNJ interagit.

*   **Objectif** : Lancer un objectif spécifique (par exemple, une quête) pour le joueur.
*   **Type d'instruction requis** : `Interaction`.

#### Configuration (JSON)

*   `"Objective"` (string, requis): L'identifiant (ID) de l'objectif à démarrer. Cet ID doit correspondre à un objectif valide existant.

#### Exemple d'utilisation (conceptuel)

```json
{
  "onInteract": {
    "action": "StartObjective",
    "Objective": "my_first_quest" // ID de l'objectif à lancer
  }
}
```

Ce builder crée une `ActionStartObjective`.

## Builders de "Sensors" (Détecteurs)

Les "Sensors" sont des conditions qui permettent à un PNJ de vérifier l'état du monde ou d'un joueur avant d'exécuter une action.

### `BuilderSensorHasTask`

Ce builder crée un "sensor" qui vérifie si le joueur en interaction a une ou plusieurs tâches spécifiques.

*   **Objectif** : Permet de créer une condition. Par exemple, "Si le joueur a la tâche 'gather_wood', alors...".
*   **Type d'instruction requis** : `Interaction`.

#### Configuration (JSON)

*   `"TasksById"` (tableau de strings, requis): La liste des identifiants (IDs) de tâches à vérifier chez le joueur.

#### Exemple d'utilisation (conceptuel)

```json
{
  "if": {
    "sensor": "HasTask",
    "TasksById": ["gather_wood", "kill_slimes"]
  },
  "then":. {
    "action": "Say",
    "Text": "Bravo, tu as bien avancé !"
  }
}
```

Ce builder crée un `SensorHasTask`.

## `BuilderDescriptorState` (État des Builders)

Chaque "builder" de PNJ possède un état qui indique sa maturité et sa stabilité. C'est une information cruciale pour savoir si un builder peut être utilisé en toute sécurité.

Il s'agit d'une `enum` Java avec les valeurs suivantes :

*   `Unknown`: L'état du builder est inconnu. À utiliser avec prudence.
*   `WorkInProgress`: Le builder est en cours de développement. Il est probablement incomplet ou instable.
*   `Experimental`: Le builder est fonctionnel mais pourrait changer radicalement dans les futures versions. Ne pas se baser dessus pour des fonctionnalités critiques.
*   `Stable`: Le builder est considéré comme stable et sûr à utiliser en production.
*   `Deprecated`: Le builder est obsolète et ne devrait plus être utilisé. Il sera probablement supprimé dans une future version.

Chaque builder expose son état via la méthode `getBuilderDescriptorState()`. Cela permet aux développeurs de décider quels builders utiliser.
