# RoleDebugFlags

L'énumération `RoleDebugFlags` fournit une collection d'indicateurs (flags) de débogage qui peuvent être activés pour observer et diagnostiquer le comportement des PNJ (Personnages Non Joueurs) associés à un certain rôle. Ces indicateurs sont très utiles pour les développeurs de mods qui créent ou modifient des comportements de PNJ et ont besoin de comprendre ce qui se passe "sous le capot".

## Valeurs de l'Énumération (Indicateurs de Débogage)

Chaque valeur représente un indicateur de débogage spécifique :

### `TraceFail`
Trace les étapes qui ont échoué dans le processus de décision du PNJ.

### `TraceSuccess`
Trace les étapes qui ont réussi dans le processus de décision du PNJ.

### `TraceSensorFailures`
Trace les défaillances des capteurs du PNJ.

### `Flock`
Trace les événements liés au comportement de groupe (flock) du PNJ.

### `FlockDamage`
Trace les événements de dégâts liés au comportement de groupe (flock) du PNJ.

### `MotionControllerSteer`
Trace l'activité de direction (steering) des contrôleurs de mouvement du PNJ.

### `Collisions`
Trace les informations de collision des contrôleurs de mouvement du PNJ.

### `BlockCollisions`
Trace les collisions jusqu'au niveau des blocs.

### `ProbeBlockCollisions`
Trace les collisions jusqu'au niveau des blocs lors de la détection (probing).

### `MotionControllerMove`
Trace l'activité de mouvement des contrôleurs de mouvement du PNJ.

### `ValidatePositions`
Valide que les positions de mouvement calculées n'entrent pas en collision avec les blocs.

### `SteeringRole`
Débogue le comportement de direction combiné (`blended steering behaviour`) du rôle, comme l'évitement et la séparation.

### `DisplayState`
Définit le nom d'affichage du PNJ pour afficher le contenu de son état interne.

### `DisplayFlock`
Définit le nom d'affichage du PNJ pour afficher l'état de son groupe (`flock`).

### `DisplayTime`
Définit le nom d'affichage du PNJ pour afficher l'heure du jour.

### `DisplayTarget`
Définit le nom d'affichage du PNJ pour afficher le type de cible verrouillée.

### `DisplayAnim`
Affiche l'état de l'animation du PNJ.

### `DisplayLightLevel`
Affiche les niveaux de lumière autour du PNJ.

### `DisplayCustom`
Affiche des informations de débogage personnalisées (générées par les composants de débogage).

### `DisplayHP`
Affiche les points de vie (HP) du PNJ sous forme numérique.

### `DisplayStamina`
Affiche l'endurance (`Stamina`) du PNJ sous forme numérique.

### `Overlaps`
Enregistre les blocs qui se chevauchent lors de la validation de la position.

### `Pathfinder`
Affiche le statut du trouveur de chemin (`pathfinder`) du PNJ.

### `DisplaySpeed`
Affiche la vitesse de l'entité.

### `DisplayFreeSlots`
Affiche les emplacements d'inventaire libres.

### `DisplayInternalId`
Affiche l'ID interne du serveur pour cette entité.

### `DisplayName`
Affiche le nom du rôle pour cette entité.

### `ValidateMath`
Valide (certains) calculs mathématiques dans le mouvement.

### `VisAvoidance`
Visualise les vecteurs d'évitement.

### `VisSeparation`
Visualise le vecteur de séparation.

### `BeaconMessages`
Active le débogage de l'envoi et de la réception des messages de balise (`beacon messages`).

## Méthodes Statiques Utilitaires

### `public static EnumSet<RoleDebugFlags> getFlags(@Nonnull String[] args)`
Parse un tableau de chaînes de caractères pour en extraire les indicateurs de débogage (`RoleDebugFlags`) et les préréglages (`presets`) correspondants.
- **Paramètres :**
    - `args` : Un tableau de chaînes de caractères représentant les indicateurs ou les noms de préréglages.
- **Retourne :** Un `EnumSet` des `RoleDebugFlags` activés.
- **Lance :** `IllegalArgumentException` si un indicateur ou un préréglage invalide est fourni.

### `public static StringBuilder getListOfFlags(@Nonnull EnumSet<RoleDebugFlags> flags, @Nonnull StringBuilder stringBuilder)`
Construit une chaîne de caractères représentant une liste d'indicateurs de débogage contenus dans l'`EnumSet` donné.
- **Paramètres :**
    - `flags` : L'`EnumSet` de `RoleDebugFlags` à lister.
    - `stringBuilder` : Un `StringBuilder` auquel ajouter la liste.
- **Retourne :** Le `StringBuilder` modifié.

### `public static StringBuilder getListOfAllFlags(@Nonnull StringBuilder stringBuilder)`
Construit une chaîne de caractères représentant la liste de tous les `RoleDebugFlags` possibles.
- **Paramètres :**
    - `stringBuilder` : Un `StringBuilder` auquel ajouter la liste.
- **Retourne :** Le `StringBuilder` modifié.

### `public static StringBuilder getListOfAllPresets(@Nonnull StringBuilder stringBuilder)`
Construit une chaîne de caractères représentant la liste de tous les noms de préréglages de débogage disponibles.
- **Paramètres :**
    - `stringBuilder` : Un `StringBuilder` auquel ajouter la liste.
- **Retourne :** Le `StringBuilder` modifié.

### `public static EnumSet<RoleDebugFlags> getPreset(String arg)`
Récupère un `EnumSet` de `RoleDebugFlags` correspondant à un préréglage de débogage nommé.
- **Paramètres :**
    - `arg` : Le nom du préréglage (par exemple, "move", "all").
- **Retourne :** Un `EnumSet` des `RoleDebugFlags` définis par le préréglage.
- **Lance :** `IllegalArgumentException` si le nom du préréglage est invalide.

### `public static boolean havePreset(String name)`
Vérifie si un préréglage de débogage avec le nom donné existe.
- **Paramètres :**
    - `name` : Le nom du préréglage.
- **Retourne :`true` si le préréglage existe, `false` sinon.
