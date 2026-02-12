# BuilderStateTransitionEdges

La classe `BuilderStateTransitionEdges` est un "builder" (constructeur) qui permet de définir les "arêtes" (`edges`) des transitions d'état pour les PNJ (Personnages Non Joueurs). Une arête de transition décrit précisément le passage d'un état (`From`) à un autre (`To`), en incluant des informations comme la priorité de la transition. Ce builder est utilisé dans le cadre de la construction de logiques d'IA complexes pour les PNJ.

## Constructeur

Le constructeur de cette classe n'est pas directement exposé pour une utilisation par les développeurs de mods. Il est géré en interne par le système de rôles des PNJ lors de la lecture des définitions.

## Méthodes

### `public String getShortDescription()`
Retourne une courte description de ce "builder" d'arêtes de transition d'état.
- **Retourne :** Une `String` décrivant l'objectif.

### `public String getLongDescription()`
Retourne une description plus détaillée de ce "builder". Actuellement, elle est identique à la courte description.
- **Retourne :** Une `String` décrivant l'objectif.

### `public StateTransitionEdges build(BuilderSupport builderSupport)`
Construit une instance de `StateTransitionEdges` à partir des configurations chargées dans ce builder. C'est la méthode qui convertit la définition des arêtes en un objet fonctionnel.
- **Paramètres :`
    - `builderSupport` : Un objet de support fournissant un contexte et des utilitaires pour le processus de construction.
- **Retourne :** Une instance de `StateTransitionEdges`.

### `public Class<StateTransitionEdges> category()`
Retourne la catégorie de la classe que ce builder construit, qui est `StateTransitionEdges.class`.
- **Retourne :** La `Class` du type `StateTransitionEdges`.

### `public BuilderDescriptorState getBuilderDescriptorState()`
Retourne l'état du descripteur de ce builder (par exemple, `Stable`, `Experimental`). Cela indique le niveau de maturité ou de support de cette fonctionnalité.
- **Retourne :** Un `BuilderDescriptorState`.

### `public boolean isEnabled(ExecutionContext context)`
Vérifie si ce "builder" est activé, en tenant compte du contexte d'exécution.
- **Paramètres :**
    - `context` : Le `ExecutionContext` actuel.
- **Retourne :** `true` si le builder est activé, `false` sinon.

### `public Builder<StateTransitionEdges> readConfig(@Nonnull JsonElement data)`
Lit la configuration des arêtes de transition d'état à partir d'un élément JSON. C'est ici que le builder extrait les informations des fichiers de définition des PNJ.
- **Paramètres :**
    - `data` : L'`JsonElement` contenant les données de configuration.
- **Retourne :** L'instance actuelle du builder, permettant des appels chaînés.

## Classe Imbriquée Statique : `StateTransitionEdges`

Cette classe représente les arêtes d'une transition d'état construites par le builder. Elle contient les indices des états de départ et d'arrivée, ainsi que la priorité de la transition.

### Méthodes

### `public int getPriority()`
Retourne la priorité de cette arête de transition. Une priorité plus élevée peut signifier que cette transition sera évaluée avant d'autres.
- **Retourne :`int`.

### `public int[] getFromStateIndices()`
Retourne un tableau des indices des états de départ pour cette arête de transition.
- **Retourne :** Un tableau d'entiers représentant les indices des états de départ.

### `public int[] getToStateIndices()`
Retourne un tableau des indices des états d'arrivée pour cette arête de transition.
- **Retourne :** Un tableau d'entiers représentant les indices des états d'arrivée.
