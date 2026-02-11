# HytaleWorldGenProvider

La classe `HytaleWorldGenProvider` sert d'implémentation par défaut pour `IWorldGenProvider`. Elle offre un moyen standardisé de configurer et de récupérer la logique de génération de monde de Hytale. Les développeurs de mods intéressés par la personnalisation ou la compréhension du fonctionnement de la génération de monde trouveront cette classe pertinente.

## Constantes

### `public static final String ID = "Hytale"`
L'identifiant unique pour ce fournisseur spécifique de génération de monde.

### `public static final BuilderCodec<HytaleWorldGenProvider> CODEC`
Une instance de `BuilderCodec` responsable de la sérialisation (sauvegarde) et de la désérialisation (chargement) des configurations de `HytaleWorldGenProvider`. Cela permet de charger ou de sauvegarder les paramètres du fournisseur (comme le nom du générateur et le chemin) dans un format persistant.

## Méthodes

### `public IWorldGen getGenerator() throws WorldGenLoadException`
C'est la méthode principale du fournisseur. Elle construit et retourne une instance de `IWorldGen`, qui est le véritable moteur de génération de monde configuré par ce fournisseur. Le comportement du générateur est déterminé par les propriétés `name` et `path` définies sur cette instance de `HytaleWorldGenProvider`.
- **Retourne :** Une instance d'`IWorldGen` prête à générer des "chunks" (morceaux de monde).
- **Lance :** `WorldGenLoadException` s'il y a un problème lors du chargement de la configuration de génération de monde ou du générateur lui-même.

### `public String toString()`
Retourne une représentation sous forme de chaîne de caractères (`String`) de cette instance de `HytaleWorldGenProvider`, incluant son nom et son chemin configurés.
- **Retourne :** Une `String` détaillant l'état actuel du fournisseur.
