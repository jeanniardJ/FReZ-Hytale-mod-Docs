# BuilderStateTransitionController

La classe `BuilderStateTransitionController` est un "builder" (constructeur) qui permet de définir un contrôleur de transitions d'état pour les PNJ (Personnages Non Joueurs). Un `StateTransitionController` est responsable de la gestion d'une liste de `StateTransition`s, qui dictent comment un PNJ passe d'un état de comportement à un autre. Ce builder facilite la lecture de ces configurations (souvent depuis des fichiers JSON) et la construction du contrôleur réel.

## Constructeur

Le constructeur de cette classe n'est pas directement exposé pour une utilisation par les développeurs de mods, car la création de `BuilderStateTransitionController` est généralement gérée par le système interne du serveur lorsqu'il lit les définitions de rôles des PNJ.

## Méthodes

### `public String getShortDescription()`
Retourne une courte description de ce "builder" de contrôleur de transitions d'état.
- **Retourne :** Une `String` décrivant l'objectif.

### `public String getLongDescription()`
Retourne une description plus détaillée de ce "builder". Actuellement, elle est identique à la courte description.
- **Retourne :** Une `String` décrivant l'objectif.

### `public StateTransitionController build(@Nonnull BuilderSupport builderSupport)`
Construit une instance de `StateTransitionController` à partir des configurations chargées dans ce builder. C'est la méthode qui convertit la définition en un contrôleur de transitions d'état fonctionnel.
- **Paramètres :**
    - `builderSupport` : Un objet de support fournissant un contexte et des utilitaires pour le processus de construction.
- **Retourne :** Une instance de `StateTransitionController`.

### `public Class<StateTransitionController> category()`
Retourne la catégorie de la classe que ce builder construit, qui est `StateTransitionController.class`.
- **Retourne :** La `Class` du type `StateTransitionController`.

### `public BuilderDescriptorState getBuilderDescriptorState()`
Retourne l'état du descripteur de ce builder (par exemple, `Stable`, `Experimental`). Cela indique le niveau de maturité ou de support de cette fonctionnalité.
- **Retourne :** Un `BuilderDescriptorState`.

### `public boolean isEnabled(ExecutionContext context)`
Vérifie si ce contrôleur de transitions d'état est activé. Pour ce builder, cela retourne toujours `true`.
- **Paramètres :**
    - `context` : Le `ExecutionContext` actuel.
- **Retourne :** `true`.

### `public Builder<StateTransitionController> readConfig(@Nonnull JsonElement data)`
Lit la configuration du contrôleur de transitions d'état à partir d'un élément JSON. C'est ici que le builder extrait les informations des fichiers de définition des PNJ.
- **Paramètres :**
    - `data` : L'`JsonElement` contenant les données de configuration.
- **Retourne :** L'instance actuelle du builder, permettant des appels chaînés.

### `public boolean validate(String configName, @Nonnull NPCLoadTimeValidationHelper validationHelper, @Nonnull ExecutionContext context, Scope globalScope, @Nonnull List<String> errors)`
Valide la configuration du contrôleur de transitions d'état. Cette méthode vérifie la cohérence des listes de transitions d'état.
- **Paramètres :`
    - `configName` : Le nom de la configuration en cours de validation.
    - `validationHelper` : Un utilitaire d'aide à la validation.
    - `context` : Le `ExecutionContext` actuel.
    - `globalScope` : L'étendue globale.
    - `errors` : Une liste où ajouter les erreurs de validation.
- **Retourne :** `true` si la configuration est valide, `false` sinon.

### `public List<BuilderStateTransition.StateTransition> getStateTransitionEntries(@Nonnull BuilderSupport support)`
Récupère la liste des entrées de transition d'état (`StateTransition`) définies par ce builder.
- **Paramètres :**
    - `support` : L'objet `BuilderSupport`.
- **Retourne :** Une `List` d'`BuilderStateTransition.StateTransition`.
