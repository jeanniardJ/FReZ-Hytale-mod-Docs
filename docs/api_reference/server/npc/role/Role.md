# Rôle (Role)

La classe `Role` est un composant essentiel qui définit le comportement et les caractéristiques d'un PNJ (Personnage Non Joueur) dans le monde de Hytale. C'est le cœur de l'intelligence artificielle et de la logique de jeu des PNJ. Un "Rôle" englobe tout, de la manière dont un PNJ se déplace et interagit, à sa gestion d'inventaire et ses capacités de combat.

Cette classe est complexe et interagit avec de nombreux sous-systèmes. La documentation se concentrera sur les méthodes publiques les plus pertinentes pour les développeurs de mods qui souhaitent comprendre ou modifier le comportement des PNJ.

## Constantes

### `public static final double INTERACTION_PLAYER_DISTANCE`
Définit la distance maximale à laquelle un joueur peut interagir avec un PNJ pour ce rôle.

### `public static final boolean DEBUG_APPLIED_FORCES`
Un indicateur de débogage (`debug flag`) qui, s'il est activé, affiche des informations sur les forces appliquées au PNJ.

## Constructeur

### `public Role(@Nonnull BuilderRole builder, @Nonnull BuilderSupport builderSupport)`
Initialise une nouvelle instance de `Role` en utilisant un `BuilderRole` et un `BuilderSupport`. Ce constructeur est principalement utilisé en interne par le système de création de PNJ pour configurer le rôle.
- **Paramètres :**
    - `builder` : Un `BuilderRole` qui spécifie les détails du rôle à construire.
    - `builderSupport` : Un objet de support fournissant des utilitaires et un contexte pour le processus de construction.

## Méthodes

### `public int getInitialMaxHealth()`
Retourne la santé maximale initiale définie pour un PNJ ayant ce rôle.
- **Retourne :** La santé maximale initiale sous forme d'entier.

### `public boolean isAvoidingEntities()`
Vérifie si le PNJ avec ce rôle est configuré pour éviter activement les autres entités.
- **Retourne :** `true` si le PNJ évite les entités, `false` sinon.

### `public double getCollisionProbeDistance()`
Retourne la distance maximale à laquelle le PNJ sonde son environnement pour détecter des collisions.
- **Retourne :** La distance de sondage de collision en `double`.

### `public boolean isApplySeparation()`
Vérifie si une force de séparation est appliquée au PNJ, ce qui aide à éviter que plusieurs PNJ ne se regroupent trop.
- **Retourne :** `true` si la séparation est appliquée, `false` sinon.

### `public double getSeparationDistance()`
Retourne la distance à laquelle le PNJ tente de se séparer des autres entités.
- **Retourne :** La distance de séparation en `double`.

### `public Instruction getRootInstruction()`
Retourne l'instruction racine du "comportement" du PNJ, qui est le point de départ de sa logique de décision. Les comportements des PNJ sont souvent définis comme des arbres d'instructions.
- **Retourne :** L'`Instruction` racine.

### `public Instruction getInteractionInstruction()`
Retourne l'instruction exécutée lorsque le PNJ interagit avec quelque chose (ou quelqu'un).
- **Retourne :** L'`Instruction` d'interaction.

### `public Instruction getDeathInstruction()`
Retourne l'instruction exécutée lorsque le PNJ meurt.
- **Retourne :** L'`Instruction` de mort.

### `public Steering getBodySteering()`
Retourne l'objet `Steering` qui contrôle les mouvements du corps du PNJ. Un `Steering` est un ensemble de forces et de directives de mouvement.
- **Retourne :** L'objet `Steering` pour le corps.

### `public Steering getHeadSteering()`
Retourne l'objet `Steering` qui contrôle les mouvements de la tête du PNJ.
- **Retourne :** L'objet `Steering` pour la tête.

### `public Set<Ref<EntityStore>> getIgnoredEntitiesForAvoidance()`
Retourne un ensemble d'entités que le PNJ doit ignorer lors de ses calculs d'évitement de collision.
- **Retourne :** Un `Set` de `Ref<EntityStore>`.

### `public String getDropListId()`
Retourne l'ID de la liste de butin (`drop list`) associée à ce rôle de PNJ. C'est ce qui détermine les objets qu'un PNJ peut laisser tomber.
- **Retourne :** L'ID de la liste de butin sous forme de `String`.

### `public String getBalanceAsset()`
Retourne l'identifiant de la ressource d'équilibrage (`balance asset`) pour ce rôle, qui peut influencer ses statistiques ou son comportement.
- **Retourne :** L'identifiant de la ressource sous forme de `String`.

### `public Map<String, String> getInteractionVars()`
Retourne une carte (`Map`) de variables spécifiques aux interactions de ce PNJ.
- **Retourne :** Une `Map<String, String>`.

### `public boolean isMemory()`
Vérifie si ce rôle de PNJ utilise un système de mémoire.
- **Retourne :** `true` si le PNJ a une mémoire, `false` sinon.

### `public String getMemoriesNameOverride()`
Retourne un nom de remplacement pour la mémoire du PNJ, si configuré.
- **Retourne :** Le nom de remplacement sous forme de `String`.

### `public String getNameTranslationKey()`
Retourne la clé de traduction (`translation key`) utilisée pour afficher le nom du rôle du PNJ dans différentes langues.
- **Retourne :** La clé de traduction sous forme de `String`.

### `public boolean isMemoriesNameOverriden()`
Vérifie si le nom de la mémoire du PNJ est remplacé.
- **Retourne :** `true` si le nom est remplacé, `false` sinon.

### `public float getSpawnLockTime()`
Retourne la durée pendant laquelle le PNJ est "verrouillé" après sa réapparition (par exemple, invulnérable ou immobile).
- **Retourne :** Le temps de verrouillage sous forme de `float`.

### `public String getAppearanceName()`
Retourne le nom de l'apparence visuelle du PNJ.
- **Retourne :** Le nom de l'apparence sous forme de `String`.

### `public MotionController getActiveMotionController()`
Retourne le `MotionController` (contrôleur de mouvement) actif du PNJ. Le contrôleur de mouvement gère la façon dont le PNJ se déplace.
- **Retourne :** Le `MotionController` actif.

### `public CombatSupport getCombatSupport()`
Retourne l'objet `CombatSupport` associé à ce rôle. Il gère la logique de combat du PNJ.
- **Retourne :** L'objet `CombatSupport`.

### `public StateSupport getStateSupport()`
Retourne l'objet `StateSupport` pour ce rôle. Il gère les différents états du PNJ.
- **Retourne :** L'objet `StateSupport`.

### `public WorldSupport getWorldSupport()`
Retourne l'objet `WorldSupport` pour ce rôle. Il gère les interactions du PNJ avec le monde.
- **Retourne :** L'objet `WorldSupport`.

### `public MarkedEntitySupport getMarkedEntitySupport()`
Retourne l'objet `MarkedEntitySupport` pour ce rôle. Il gère le ciblage d'entités spécifiques.
- **Retourne :** L'objet `MarkedEntitySupport`.

### `public PositionCache getPositionCache()`
Retourne l'`PositionCache` pour ce rôle. Il stocke des informations sur la position du PNJ et de son environnement.
- **Retourne :** L'`PositionCache`.

### `public EntitySupport getEntitySupport()`
Retourne l'objet `EntitySupport` pour ce rôle. Il fournit des utilitaires pour interagir avec l'entité PNJ.
- **Retourne :** L'objet `EntitySupport`.

### `public DebugSupport getDebugSupport()`
Retourne l'objet `DebugSupport` pour ce rôle. Il fournit des outils de débogage spécifiques au rôle.
- **Retourne :** L'objet `DebugSupport`.

### `public boolean isRoleChangeRequested()`
Vérifie si un changement de rôle a été demandé pour ce PNJ.
- **Retourne :** `true` si un changement de rôle est demandé, `false` sinon.

### `public void setRoleChangeRequested()`
Demande un changement de rôle pour le PNJ.

### `public boolean setActiveMotionController(@Nullable Ref<EntityStore> ref, @Nonnull NPCEntity npcComponent, @Nonnull String name, @Nullable ComponentAccessor<EntityStore> componentAccessor)`
Définit le `MotionController` actif du PNJ par son nom.
- **Paramètres :**
    - `ref` : La `Ref` vers l'entité du PNJ.
    - `npcComponent` : Le composant `NPCEntity`.
    - `name` : Le nom du `MotionController` à activer.
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** `true` si le contrôleur a été activé avec succès, `false` sinon.

### `public boolean isInvulnerable()`
Vérifie si le PNJ est invulnérable.
- **Retourne :** `true` si le PNJ est invulnérable, `false` sinon.

### `public boolean isBreathesInAir()`
Vérifie si le PNJ respire dans l'air.
- **Retourne :** `true` si le PNJ respire dans l'air, `false` sinon.

### `public boolean isBreathesInWater()`
Vérifie si le PNJ respire dans l'eau.
- **Retourne :** `true` si le PNJ respire dans l'eau, `false` sinon.

### `public double getInertia()`
Retourne l'inertie du PNJ, qui influence sa résistance aux changements de mouvement.
- **Retourne :** La valeur d'inertie en `double`.

### `public double getKnockbackScale()`
Retourne l'échelle de recul (`knockback scale`) du PNJ, qui détermine l'ampleur du recul qu'il subit.
- **Retourne :** L'échelle de recul en `double`.

### `public boolean canBreathe(@Nonnull BlockMaterial breathingMaterial, int fluidId)`
Vérifie si le PNJ peut respirer dans un matériau donné et un fluide donné. L'invulnérabilité peut affecter ce résultat.
- **Paramètres :**
    - `breathingMaterial` : Le `BlockMaterial` dans lequel le PNJ tente de respirer.
    - `fluidId` : L'ID du fluide (0 pour l'air).
- **Retourne :** `true` si le PNJ peut respirer, `false` sinon.

### `public boolean isPickupDropOnDeath()`
Vérifie si le PNJ lâche son butin (`drops`) à sa mort.
- **Retourne :** `true` si le PNJ lâche du butin, `false` sinon.

### `public boolean requiresLeashPosition()`
Vérifie si le PNJ nécessite une position d'attache (`leash position`).
- **Retourne :** `true` si une position d'attache est requise, `false` sinon.

### `public void setMarkedTarget(@Nonnull String targetSlot, @Nonnull Ref<EntityStore> target)`
Définit une cible marquée pour le PNJ, associée à un `targetSlot` spécifique.
- **Paramètres :**
    - `targetSlot` : L'emplacement de la cible marquée.
    - `target` : La `Ref<EntityStore>` de l'entité cible.

### `public boolean isFriendly(@Nonnull Ref<EntityStore> ref, @Nonnull ComponentAccessor<EntityStore> componentAccessor)`
Vérifie si le PNJ est considéré comme amical.
- **Paramètres :**
    - `ref` : La `Ref` vers l'entité du PNJ.
    - `componentAccessor` : Un accesseur pour les composants d'entité.
- **Retourne :** `true` si le PNJ est amical, `false` sinon.

### `public boolean isIgnoredForAvoidance(@Nonnull Ref<EntityStore> entityReference)`
Vérifie si une entité spécifique est ignorée par le PNJ pour les calculs d'évitement.
- **Paramètres :**
    - `entityReference` : La `Ref<EntityStore>` de l'entité à vérifier.
- **Retourne :** `true` si l'entité est ignorée, `false` sinon.

### `public AvoidanceMode getAvoidanceMode()`
Retourne le mode d'évitement (`AvoidanceMode`) configuré pour ce PNJ.
- **Retourne :** L'`AvoidanceMode`.

### `public double getCollisionRadius()`
Retourne le rayon de collision du PNJ.
- **Retourne :** Le rayon de collision en `double`.

### `public int[] getFlockSpawnTypes()`
Retourne les types de "flock spawn" (apparition en groupe) configurés pour ce PNJ.
- **Retourne :** Un tableau d'entiers représentant les types d'apparition en groupe.

### `public String[] getFlockAllowedRoles()`
Retourne un tableau des rôles de PNJ autorisés dans le même groupe (`flock`) que ce PNJ.
- **Retourne :** Un tableau de `String` des rôles autorisés.

### `public boolean isFlockSpawnTypesRandom()`
Vérifie si les types d'apparition en groupe sont aléatoires.
- **Retourne :`true` si les types sont aléatoires, `false` sinon.

### `public boolean isCanLeadFlock()`
Vérifie si le PNJ peut diriger un groupe (`flock`).
- **Retourne :`true` si le PNJ peut diriger, `false` sinon.

### `public double getFlockInfluenceRange()`
Retourne la portée d'influence du groupe (`flock influence range`) pour ce PNJ.
- **Retourne :`double`.

### `public double getDeathAnimationTime()`
Retourne la durée de l'animation de mort du PNJ.
- **Retourne :`double`.

### `public String getDeathInteraction()`
Retourne l'ID de l'interaction de mort configurée pour le PNJ.
- **Retourne :`String`.

### `public float getDespawnAnimationTime()`
Retourne la durée de l'animation de disparition du PNJ.
- **Retourne :`float`.

### `public boolean hasReachedTerminalAction()`
Vérifie si le PNJ a atteint une action terminale (une action qui met fin à son comportement ou à son cycle de vie).
- **Retourne :`true` si une action terminale est atteinte, `false` sinon.

### `public boolean isFlagSet(int index)`
Vérifie si un indicateur (`flag`) spécifique est défini pour le PNJ. Les indicateurs sont des booléens internes utilisés pour contrôler le comportement.
- **Paramètres :**
    - `index` : L'index de l'indicateur à vérifier.
- **Retourne :`true` si l'indicateur est défini, `false` sinon.

### `public boolean isBackingAway()`
Vérifie si le PNJ est en train de reculer.
- **Retourne :`true` si le PNJ recule, `false` sinon.

### `public String getRoleName()`
Retourne le nom de ce rôle de PNJ.
- **Retourne :`String`.

### `public int getRoleIndex()`
Retourne l'index interne de ce rôle.
- **Retourne :`int`.

### `public boolean isCorpseStaysInFlock()`
Vérifie si le cadavre du PNJ reste dans le groupe (`flock`) après sa mort.
- **Retourne :`true` si le cadavre reste, `false` sinon.

### `public RoleStats getRoleStats()`
Retourne les statistiques de rôle (`RoleStats`) pour ce PNJ.
- **Retourne :`RoleStats`.

## Énumération Imbriquée : `AvoidanceMode`

L'énumération `AvoidanceMode` définit les différentes stratégies d'évitement qu'un PNJ peut utiliser pour éviter les collisions avec d'autres entités.

### Valeurs de l'Énumération

### `Slowdown` ("Seulement ralentir le PNJ")
Le PNJ ralentira pour éviter les collisions.

### `Evade` ("Seulement éviter")
Le PNJ tentera d'éviter activement les obstacles.

### `Any` ("Tout évitement autorisé")
Le PNJ peut utiliser n'importe quelle stratégie d'évitement disponible.

## Interface Imbriquée : `DeferredAction`

L'interface `DeferredAction` représente une action qui sera exécutée ultérieurement (différée) par le système de rôle du PNJ.
