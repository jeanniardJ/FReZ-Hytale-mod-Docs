# CommandManager

La classe `CommandManager` est l'autorité centrale pour la gestion et l'exécution des commandes au sein du serveur Hytale. Elle permet l'enregistrement de nouvelles commandes, la gestion de leur exécution depuis diverses sources (comme les joueurs ou la console) et la gestion des permissions liées aux commandes. Les développeurs de mods interagiront avec ce gestionnaire pour ajouter des commandes personnalisées à leur serveur.

## Méthodes Statiques

### `public static CommandManager get()`
Récupère l'instance "singleton" du `CommandManager`. C'est le moyen principal d'accéder aux fonctionnalités de gestion des commandes.
- **Retourne :** L'instance unique du `CommandManager`.

## Méthodes

### `public Map<String, AbstractCommand> getCommandRegistration()`
Retourne une carte (`Map`) immuable de toutes les commandes actuellement enregistrées. Les clés sont les noms des commandes (insensible à la casse) et les valeurs sont leurs instances `AbstractCommand` correspondantes. Cela peut être utile pour inspecter les commandes disponibles.
- **Retourne :** Une `Map` des noms de commandes vers des objets `AbstractCommand`.

### `public Map<String, Set<String>> createVirtualPermissionGroups()`
Génère une carte des groupes de permissions virtuels basés sur les permissions définies par les commandes enregistrées. Ceci est principalement utilisé pour la gestion interne des permissions et l'introspection (examiner la structure interne).
- **Retourne :** Une `Map` où les clés sont les noms des groupes de permissions et les valeurs sont des ensembles (`Set`) de permissions associées.

### `public void registerSystemCommand(@Nonnull AbstractCommand command)`
Enregistre une commande comme une commande de niveau système. Ces commandes font généralement partie des fonctionnalités de base du serveur, mais peuvent potentiellement être étendues par les mods.
- **Paramètres :**
    - `command` : L'instance `AbstractCommand` à enregistrer.

### `public CommandRegistration register(@Nonnull AbstractCommand command)`
Enregistre une commande personnalisée auprès du serveur. C'est la méthode principale que les développeurs de mods utiliseraient pour ajouter leurs propres commandes. Après un enregistrement réussi, un objet `CommandRegistration` est retourné, qui peut être utilisé pour gérer le cycle de vie de la commande (par exemple, la désenregistrer).
- **Paramètres :**
    - `command` : L'instance `AbstractCommand` représentant la commande à enregistrer.
- **Retourne :** Un objet `CommandRegistration` si la commande a été enregistrée avec succès, ou `null` si l'enregistrement a échoué (par exemple, en raison d'un nom invalide ou d'un conflit).

### `public CompletableFuture<Void> handleCommand(@Nonnull PlayerRef playerRef, @Nonnull String command)`
Envoie une chaîne de commande pour exécution, généralement à l'origine d'un joueur. La commande est traitée de manière asynchrone (en arrière-plan), ce qui signifie que le serveur peut continuer à faire d'autres choses pendant que la commande est traitée.
- **Paramètres :**
    - `playerRef` : Un objet `PlayerRef` représentant le joueur qui a émis la commande.
    - `command` : La chaîne de commande complète, incluant le nom de la commande et ses arguments (par exemple, "teleport PlayerName 100 60 200").
- **Retourne :** Un `CompletableFuture<Void>` qui se termine lorsque la commande a fini son exécution ou qu'une erreur s'est produite.

### `public CompletableFuture<Void> handleCommand(@Nonnull CommandSender commandSender, @Nonnull String commandString)`
Envoie une chaîne de commande pour exécution, provenant de n'importe quel `CommandSender` (par exemple, un joueur, la console ou un autre plugin). La commande est traitée de manière asynchrone.
- **Paramètres :**
    - `commandSender` : Le `CommandSender` qui a émis la commande.
    - `commandString` : La chaîne de commande complète.
- **Retourne :** Un `CompletableFuture<Void>` qui se termine lorsque la commande a fini son exécution ou qu'une erreur s'est produite.

### `public CompletableFuture<Void> handleCommands(@Nonnull CommandSender sender, @Nonnull Deque<String> commands)`
Envoie une séquence de chaînes de commande pour exécution par un `CommandSender`. Chaque commande dans la `Deque` (une file d'attente à double extrémité) est traitée séquentiellement.
- **Paramètres :**
    - `sender` : Le `CommandSender` qui a émis les commandes.
    - `commands` : Une `Deque` de chaînes de commande à exécuter.
- **Retourne :** Un `CompletableFuture<Void>` qui se termine lorsque toutes les commandes de la séquence ont fini leur exécution.

### `public String getName()`
Retourne le nom du propriétaire de ces commandes. Dans le contexte du serveur Hytale, cela retourne généralement "HytaleServer".
- **Retourne :** Le nom du propriétaire sous forme de `String`.
