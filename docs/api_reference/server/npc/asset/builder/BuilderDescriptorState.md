# BuilderDescriptorState

L'énumération `BuilderDescriptorState` définit les différents états ou niveaux de maturité qu'un "Builder Descriptor" peut avoir. Ces états sont utilisés pour indiquer le statut de développement ou de support d'une certaine fonctionnalité ou ressource liée aux PNJ (Personnages Non Joueurs) et à la construction.

## Valeurs de l'Énumération

### `Unknown`
L'état du descripteur est inconnu ou non spécifié.

### `WorkInProgress`
Indique que la fonctionnalité ou la ressource est en cours de développement et peut être sujette à des changements fréquents ou majeurs.

### `Experimental`
Signale que la fonctionnalité est expérimentale. Elle est disponible pour les tests, mais n'est pas garantie d'être stable ou d'être conservée dans les versions futures.

### `Stable`
Indique que la fonctionnalité ou la ressource est considérée comme stable et prête à être utilisée en production.

### `Deprecated`
Signale que la fonctionnalité ou la ressource est obsolète. Elle peut encore fonctionner, mais son utilisation est déconseillée et elle pourrait être supprimée dans les futures mises à jour. Les développeurs devraient chercher des alternatives.
