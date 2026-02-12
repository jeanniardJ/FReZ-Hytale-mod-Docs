# SubTypeTypeAdapterFactory

La classe `SubTypeTypeAdapterFactory` est une fabrique d'adaptateurs de type (`TypeAdapterFactory`) pour la bibliothèque Gson (utilisée pour la sérialisation/désérialisation JSON). Elle est conçue pour gérer la sérialisation et la désérialisation d'objets qui peuvent avoir différents "sous-types" (par exemple, différentes implémentations d'une même interface ou classe de base), où le sous-type est indiqué par un champ spécifique dans le JSON.

Ceci est particulièrement utile dans les configurations où une structure de données JSON peut représenter plusieurs types d'objets distincts, mais liés.

## Constructeur

### `private SubTypeTypeAdapterFactory(Class<?> baseClassType, String typeFieldName)`
Le constructeur est privé, ce qui signifie que vous ne pouvez pas créer directement une instance de cette classe. Utilisez la méthode statique `of()` pour obtenir une instance.

## Méthodes Statiques

### `public static SubTypeTypeAdapterFactory of(Class<?> baseClass, String typeFieldName)`
Crée une nouvelle instance de `SubTypeTypeAdapterFactory`. C'est la méthode recommandée pour commencer à configurer cette fabrique.
- **Paramètres :**
    - `baseClass` : La classe de base ou l'interface que les sous-types étendront ou implémenteront.
    - `typeFieldName` : Le nom du champ JSON qui contiendra le nom du sous-type (par exemple, "type").
- **Retourne :** Une nouvelle instance de `SubTypeTypeAdapterFactory`.

## Méthodes

### `public SubTypeTypeAdapterFactory registerSubType(Class<?> clazz, String name)`
Enregistre un sous-type spécifique auprès de la fabrique. Vous devez associer une classe de sous-type à un nom qui sera utilisé comme valeur dans le champ `typeFieldName` du JSON.
- **Paramètres :**
    - `clazz` : La classe du sous-type à enregistrer (par exemple, `MonSousTypeImpl.class`).
    - `name` : Le nom qui représentera ce sous-type dans le JSON (par exemple, "mon_sous_type").
- **Lance :** `IllegalArgumentException` si la classe ou le nom est déjà enregistré.
- **Retourne :** L'instance actuelle de `SubTypeTypeAdapterFactory`, ce qui permet d'enchaîner les appels (`fluent API`).

### `public <T> TypeAdapter<T> create(@Nonnull Gson gson, @Nonnull TypeToken<T> type)`
Méthode interne de `TypeAdapterFactory`. Elle est appelée par Gson pour créer un `TypeAdapter` pour un type donné. Vous n'aurez généralement pas besoin d'appeler cette méthode directement.
- **Paramètres :**
    - `gson` : L'instance de `Gson` utilisée.
    - `type` : Le `TypeToken` de l'objet à sérialiser/désérialiser.
- **Retourne :** Un `TypeAdapter` pour le type demandé, ou `null` si cette fabrique ne peut pas gérer le type.

## Utilisation pour les Développeurs

Bien que la compréhension complète de `TypeAdapterFactory` soit avancée, les développeurs de mods pourraient rencontrer cette classe s'ils travaillent avec des fichiers de configuration JSON complexes qui utilisent des structures polymorphiques (où le type d'un objet peut changer dynamiquement). L'important est de savoir que c'est un mécanisme pour Gson permettant de lire et écrire différents objets en fonction d'un champ "type" dans le JSON.
