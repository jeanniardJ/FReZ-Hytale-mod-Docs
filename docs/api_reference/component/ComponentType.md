# ComponentType

La classe `ComponentType` est un élément fondamental du système d'entités basé sur les composants (ECS - Entity Component System) de Hytale. Chaque type de composant (par exemple, un composant de position, un composant d'inventaire) possède une instance unique de `ComponentType`. Cette instance agit comme un identifiant et contient des métadonnées sur le type de composant, permettant au système ECS de gérer efficacement les composants.

## Constantes

### `public static final ComponentType[] EMPTY_ARRAY`
Un tableau vide d'objets `ComponentType`. Utile pour éviter les `NullPointerException` lorsque vous travaillez avec des tableaux de types de composants.

## Méthodes

### `public ComponentRegistry<ECS_TYPE> getRegistry()`
Retourne le `ComponentRegistry` auquel ce `ComponentType` appartient. Un `ComponentRegistry` est comme un répertoire qui gère tous les types de composants pour un certain système (par exemple, pour les entités ou les chunks).
- **Retourne :** Le `ComponentRegistry` associé.

### `public Class<? super T> getTypeClass()`
Retourne la classe Java réelle que ce `ComponentType` représente. Par exemple, si votre composant est une classe `MonComposantDePosition`, cette méthode retournera `MonComposantDePosition.class`.
- **Retourne :** La `Class` Java du type de composant.

### `public int getIndex()`
Retourne l'index interne de ce type de composant au sein de son registre. Cet index est utilisé en interne par le système ECS pour des raisons de performance et n'est généralement pas pertinent pour les développeurs de mods.
- **Retourne :** L'index interne.

### `public boolean test(@Nonnull Archetype<ECS_TYPE> archetype)`
Vérifie si un `Archetype` donné contient ce type de composant. Un `Archetype` décrit un ensemble unique de types de composants qu'une entité peut posséder.
- **Paramètres :**
    - `archetype` : L'`Archetype` à tester.
- **Retourne :** `true` si l'`Archetype` contient ce `ComponentType`, `false` sinon.

### `public boolean requiresComponentType(ComponentType<ECS_TYPE, ?> componentType)`
Vérifie si ce `ComponentType` est le même que le `componentType` donné. C'est équivalent à utiliser la méthode `equals()` pour comparer deux `ComponentType`s.
- **Paramètres :**
    - `componentType` : Le `ComponentType` à comparer.
- **Retourne :** `true` si les types de composants sont identiques, `false` sinon.

### `public void validateRegistry(@Nonnull ComponentRegistry<ECS_TYPE> registry)`
Valide que ce `ComponentType` appartient bien au `ComponentRegistry` fourni. C'est une vérification interne pour s'assurer de la cohérence du système.
- **Paramètres :**
    - `registry` : Le `ComponentRegistry` à valider.
- **Lance :** `IllegalArgumentException` si le `ComponentType` appartient à un registre différent.

### `public void validate()`
Valide que ce `ComponentType` est toujours valide et n'a pas été invalidé (par exemple, lors du déchargement d'un plugin). C'est une vérification interne.
- **Lance :** `IllegalStateException` si le `ComponentType` est invalide.

### `public int compareTo(@Nonnull ComponentType<ECS_TYPE, ?> o)`
Compare ce `ComponentType` à un autre en fonction de leurs index internes. Cette méthode est utilisée pour le tri et n'est généralement pas directement utilisée par les développeurs de mods.
- **Paramètres :**
    - `o` : L'autre `ComponentType` à comparer.
- **Retourne :** Un entier négatif, zéro ou positif si cet objet est respectivement inférieur, égal ou supérieur à l'objet spécifié.

### `public boolean equals(@Nullable Object o)`
Vérifie l'égalité de ce `ComponentType` avec un autre objet. Deux `ComponentType`s sont égaux s'ils appartiennent au même registre et ont le même index interne.
- **Paramètres :**
    - `o` : L'objet à comparer.
- **Retourne :** `true` si les objets sont égaux, `false` sinon.

### `public int hashCode()`
Retourne le code de hachage (`hash code`) pour ce `ComponentType`.
- **Retourne :** Un entier représentant le code de hachage.

### `public String toString()`
Retourne une représentation textuelle (`String`) de ce `ComponentType`, utile pour le débogage.
- **Retourne :** Une `String` décrivant le `ComponentType`.
