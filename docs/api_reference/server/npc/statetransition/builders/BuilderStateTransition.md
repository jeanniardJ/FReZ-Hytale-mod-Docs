# BuilderStateTransition

La classe `BuilderStateTransition` est un "builder" (constructeur) qui aide à définir les transitions d'état pour les PNJ (Personnages Non Joueurs). Dans l'intelligence artificielle des PNJ, les transitions d'état sont des règles qui déterminent comment un PNJ passe d'un comportement à un autre (par exemple, d'un état de "patrouille" à un état de "combat"). Ce builder est responsable de la lecture des configurations (souvent depuis des fichiers JSON) et de la construction de la logique de transition d'état réelle.

## Constructeur

Le constructeur de cette classe n'est pas directement exposé pour une utilisation par les développeurs de mods, car la création de `BuilderStateTransition` est généralement gérée par le système interne du serveur lorsqu'il lit les définitions de rôles des PNJ.

## Méthodes

### `public String getShortDescription()`
Retourne une courte description de ce "builder" de transition d'état.
- **Retourne :** Une `String` décrivant l'objectif.

### `public String getLongDescription()`
Retourne une description plus détaillée de ce "builder" de transition d'état. Actuellement, elle est identique à la courte description.
- **Retourne :** Une `String` décrivant l'objectif.

### `public StateTransition build(@Nonnull BuilderSupport builderSupport)`
Construit une instance de `StateTransition` à partir des configurations chargées dans ce builder. C'est la méthode qui convertit la définition en une logique de transition d'état fonctionnelle.
- **Paramètres :**
    - `builderSupport` : Un objet de support fournissant un contexte et des utilitaires pour le processus de construction.
- **Retourne :** Une instance de `StateTransition`.

### `public Class<StateTransition> category()`
Retourne la catégorie de la classe que ce builder construit, qui est `StateTransition.class`.
- **Retourne :** La `Class` du type `StateTransition`.

### `public BuilderDescriptorState getBuilderDescriptorState()`
Retourne l'état du descripteur de ce builder (par exemple, `Stable`, `Experimental`). Cela indique le niveau de maturité ou de support de cette fonctionnalité.
- **Retourne :** Un `BuilderDescriptorState`.

### `public boolean isEnabled(ExecutionContext context)`
Vérifie si cette transition d'état est activée, en tenant compte du contexte d'exécution.
- **Paramètres :**
    - `context` : Le `ExecutionContext` actuel.
- **Retourne :** `true` si la transition est activée, `false` sinon.

### `public Builder<StateTransition> readConfig(@Nonnull JsonElement data)`
Lit la configuration de la transition d'état à partir d'un élément JSON. C'est ici que le builder extrait les informations des fichiers de définition des PNJ.
- **Paramètres :**
    - `data` : L'`JsonElement` contenant les données de configuration.
- **Retourne :** L'instance actuelle du builder, permettant des appels chaînés.

### `public boolean validate(String configName, @Nonnull NPCLoadTimeValidationHelper validationHelper, @Nonnull ExecutionContext context, Scope globalScope, @Nonnull List<String> errors)`
Valide la configuration de la transition d'état. Cette méthode vérifie la cohérence des règles de transition et des actions associées.
- **Paramètres :**
    - `configName` : Le nom de la configuration en cours de validation.
    - `validationHelper` : Un utilitaire d'aide à la validation.
    - `context` : Le `ExecutionContext` actuel.
    - `globalScope` : L'étendue globale.
    - `errors` : Une liste où ajouter les erreurs de validation.
- **Retourne :** `true` si la configuration est valide, `false` sinon.

### `public List<BuilderStateTransitionEdges.StateTransitionEdges> getStateTransitionEdges(@Nonnull BuilderSupport support)`
Récupère la liste des arêtes de transition d'état définies par ce builder. Une arête de transition définit le passage d'un état à un autre.
- **Paramètres :**
    - `support` : L'objet `BuilderSupport`.
- **Retourne :** Une `List` d'`BuilderStateTransitionEdges.StateTransitionEdges`.

### `public ActionList getActionList(@Nonnull BuilderSupport builderSupport)`
Récupère la liste des actions à exécuter lors de cette transition d'état.
- **Paramètres :**
    - `builderSupport` : L'objet `BuilderSupport`.
- **Retourne :** Une `ActionList`.

## Classe Imbriquée Statique : `StateTransition`

Cette classe représente la transition d'état construite par le builder. Elle contient les arêtes de transition et les actions associées.

### Méthodes

### `public List<BuilderStateTransitionEdges.StateTransitionEdges> getStateTransitionEdges()`
Retourne la liste des arêtes de transition d'état.
- **Retourne :** Une `List` d'`BuilderStateTransitionEdges.StateTransitionEdges`.

### `public ActionList getActions()`
Retourne la liste des actions à exécuter lors de cette transition.
- **Retourne :** Une `ActionList`.
