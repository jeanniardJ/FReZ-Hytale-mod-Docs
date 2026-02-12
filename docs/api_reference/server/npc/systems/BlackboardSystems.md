# BlackboardSystems

La classe `BlackboardSystems` semble servir de conteneur pour divers systèmes liés aux PNJ (Personnages Non Joueurs) qui interagissent avec le "Blackboard" de l'IA. Le "Blackboard" est un concept courant en intelligence artificielle de jeu, agissant comme un espace partagé où les PNJ peuvent lire et écrire des informations pour coordonner leurs actions et leurs comportements.

**Note sur le Fichier Source :** Le fichier source de cette classe contient l'annotation `// INTERNAL ERROR //` en première ligne. Cela indique que ce fichier a été généré par un décompilateur et que le contenu original du code source n'a pas pu être entièrement reconstruit ou que l'outil a ajouté ce marqueur. Par conséquent, les détails internes de cette classe ne sont pas entièrement disponibles ou fiables via cette version décompilée.

## Systèmes Imbriqués (Probable)

Bien que le code interne ne soit pas lisible, la convention de nommage `BlackboardSystems` suggère qu'elle contient des classes imbriquées statiques (souvent d'autres systèmes ECS) qui sont responsables de la lecture ou de l'écriture d'informations sur le Blackboard pour influencer le comportement des PNJ.

En général, les classes de ce type :
- Définissent des `Query`s (requêtes) pour identifier les entités pertinentes.
- Implémentent des méthodes de cycle de vie (`onComponentAdded`, `onTick`, etc.) pour réagir aux changements ou effectuer des actions.
- Interagissent avec le `Blackboard` pour obtenir des informations sur le monde, les joueurs ou d'autres PNJ, et pour y déposer des résultats.

Étant donné que le code est décompilé avec une erreur interne, il est difficile d'en fournir une documentation plus détaillée sans accès au code source original ou à une décompilation plus propre.
