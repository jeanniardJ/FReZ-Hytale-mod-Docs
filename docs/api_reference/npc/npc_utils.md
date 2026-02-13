# 🛠️ Utilitaires pour PNJ (NPC Utils)

Cette section décrit diverses classes utilitaires utilisées dans le système de PNJ.

## `Alarm`

`Alarm` est une petite classe utilitaire qui permet de définir un "réveil" ou une "alarme" pour un PNJ. C'est un moyen de mémoriser un point précis dans le temps pour y réagir plus tard.

*   **Héritage** : `PersistentParameter<Instant>`
    *   Cela signifie que la valeur de l'alarme est **persistante**. Si le PNJ est déchargé du monde puis rechargé, il se souviendra de l'heure de son alarme.
*   **Objectif** : Permettre à un PNJ de programmer une vérification ou une action à un moment futur.

### Utilisation

Une alarme encapsule un `Instant` (un moment précis, à la nanoseconde près).

1.  **Définir l'alarme** :
    ```java
    // Dans une action de PNJ
    Alarm myAlarm = new Alarm();
    // Met l'alarme pour dans 10 secondes
    myAlarm.set(Instant.now().plusSeconds(10));
    ```

2.  **Vérifier l'alarme** :
    ```java
    // Dans un "sensor" (détecteur) de PNJ, à chaque tick
    if (myAlarm.isSet() && myAlarm.hasPassed(Instant.now())) {
        // L'alarme s'est déclenchée !
        // Faire quelque chose...
        
        // On peut la "réinitialiser" en la mettant à null
        myAlarm.set(null); 
    }
    ```

### Cas d'usage typiques

*   **Temporisation** : "Après avoir parlé au joueur, attendre 30 secondes avant de retourner patrouiller."
*   **Cycles jour/nuit** : "Vérifier si c'est le crépuscule. Si oui, mettre une alarme pour le début de la nuit pour commencer à chasser."
*   **Cooldowns** : "Après avoir utilisé une compétence spéciale, mettre une alarme pour 1 minute pour savoir quand je peux la réutiliser."

Comme la classe a un `CODEC`, elle peut être facilement sauvegardée et chargée avec l'état du PNJ.

## `NPCPhysicsMath`

Cette classe est une boîte à outils statique remplie de fonctions mathématiques complexes, spécifiquement conçues pour la physique et le mouvement des PNJ. Un développeur n'a généralement pas besoin d'implémenter ces calculs lui-même, mais il est utile de savoir que cette classe existe pour des besoins avancés.

Les fonctions peuvent être regroupées en plusieurs catégories :

### 1. Mathématiques Vectorielles Avancées

Ces fonctions vont au-delà des opérations de base des vecteurs.

*   `headingFromDirection`, `pitchFromDirection`: Calculent l'orientation (yaw) et l'inclinaison (pitch) à partir d'un vecteur de direction.
*   `cosAngleBetweenVectors`: Calcule le cosinus de l'angle entre deux vecteurs, ce qui est plus performant que de calculer l'angle lui-même.
*   `projection`, `rejection`: Projettent un vecteur sur un autre, ou calculent la partie du vecteur qui est perpendiculaire. Utile pour la physique des collisions.

### 2. Détection et Champ de Vision

Ces fonctions aident un PNJ à "voir" le monde.

*   `inViewSector`, `isInViewCone`: Vérifient si un point est à l'intérieur du cône de vision d'un PNJ. C'est la base de la plupart des systèmes de détection d'IA.

### 3. Tests d'Intersection et Collision

Fonctions de bas niveau pour la détection de collisions.

*   `intersectLineSphere`: Calcule si et où une ligne coupe une sphère.
*   `intersectSweptSpheres`: Calcule si et quand deux sphères en mouvement vont se heurter. C'est fondamental pour l'évitement d'entités.

### 4. Simulation de Mouvement

Ces fonctions permettent de simuler des mouvements réalistes.

*   `accelerate`, `deccelerateToStop`: Simulent une accélération ou une décélération simple.
*   `accelerateDrag`, `gravityDrag`: Des fonctions plus complexes qui simulent l'accélération en prenant en compte la résistance de l'air (`drag`) et la gravité.
*   `jumpParameters`: Calcule la vélocité initiale nécessaire pour qu'un PNJ saute et atterrisse à une position cible.

### 5. Interaction avec le Monde

*   `heightOverGround`: Calcule la hauteur d'un point par rapport au sol solide en dessous.
*   `blockEmptySpace`: Calcule l'espace vide dans un bloc, en tenant compte de sa boîte de collision.

En résumé, `NPCPhysicsMath` est une classe fondamentale qui fournit les briques de base mathématiques pour presque tous les aspects du mouvement et de la perception de l'environnement des PNJ.

## `Timer`

`Timer` est une classe utilitaire plus complexe qui implémente un compte à rebours. Contrairement à `Alarm` qui se déclenche à un instant `T`, `Timer` décompte une durée.

*   **Interface** : `Tickable`. Cela signifie qu'un objet extérieur (généralement le `Role` ou une `Action`) doit appeler sa méthode `tick(dt)` à chaque frame pour que le temps s'écoule.
*   **Objectif** : Gérer des durées, des cooldowns, ou des actions à intervalle régulier.

### États et Contrôle

Un `Timer` a sa propre machine à états interne :
*   `RUNNING`: Le timer est en train de décompter.
*   `PAUSED`: Le décompte est gelé.
*   `STOPPED`: Le timer est terminé et à zéro.

On peut le contrôler avec des méthodes comme `start()`, `stop()`, `pause()`, `resume()`, et `restart()`.

### Configuration et Utilisation

1.  **Démarrage** : On démarre un timer avec des valeurs précises.
    ```java
    // Créer un nouveau timer
    Timer myTimer = new Timer();
    
    // Le démarrer. Il commencera avec une valeur aléatoire entre 5 et 10 secondes.
    // Il se réinitialisera à une valeur entre 8 et 12 secondes.
    // Il décompte de 1.0 par seconde.
    // Il se répète automatiquement.
    myTimer.start(5.0, 10.0, 8.0, 12.0, 1.0, true);
    ```

2.  **Mise à jour (tick)** : Dans une méthode qui est appelée à chaque tick...
    ```java
    // dt est le delta time, le temps écoulé depuis la dernière frame.
    myTimer.tick(dt); 
    ```

3.  **Vérification** :
    ```java
    // On peut vérifier si le timer est à zéro.
    // Note : si le timer se répète, il ne sera jamais "stopped" de lui-même.
    // Il faut vérifier la valeur ou un autre indicateur.
    if (myTimer.isStopped()) {
        // Le temps est écoulé !
    }
    
    // Obtenir la valeur restante
    double timeLeft = myTimer.getValue();
    ```

### Cas d'usage typiques

*   **Comportements à intervalle** : "Toutes les 10 à 20 secondes, regarde autour de toi." (On utiliserait un timer qui se répète).
*   **Durée d'une action** : "Reste assis sur ce banc pendant 30 secondes." (Un timer qui ne se répète pas).
*   **Cooldown de compétence** : "Après avoir lancé une boule de feu, attends 5 secondes avant de pouvoir en lancer une autre."

## `ViewTest`

Cette `enum` est utilisée pour spécifier le type de test de vision à effectuer lors de la détection d'entités par un PNJ.

Les PNJ n'ont pas des "yeux" qui voient à 360 degrés. Leurs "sensors" (détecteurs) utilisent souvent un test de vision pour déterminer si une cible est dans leur champ de vision. Cette `enum` permet de choisir la forme de ce champ de vision.

*   `NONE`: Aucun test de vision n'est effectué. La détection se base uniquement sur la distance.
*   `VIEW_SECTOR`: Effectue un test en 2D (sur le plan horizontal X/Z). C'est comme un secteur de cercle qui part du PNJ. C'est moins cher en performance mais ne prend pas en compte la hauteur. Utile pour des PNJ terrestres simples.
*   `VIEW_CONE`: Effectue un test en 3D complet, en utilisant un cône de vision. C'est plus précis, indispensable pour des PNJ qui peuvent voler ou se battre sur des terrains accidentés, mais légèrement plus coûteux en performance.

Le choix entre `VIEW_SECTOR` et `VIEW_CONE` est un compromis classique en IA de jeu entre performance et précision.
