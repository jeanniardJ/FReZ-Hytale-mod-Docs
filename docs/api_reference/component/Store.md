# Classe : Store<ECS_TYPE>

La classe `Store` est un élément **fondamental** du système ECS (Entity Component System) de Hytale. C'est le "cœur" où sont stockées et gérées toutes les entités de votre monde, ainsi que les composants qui les définissent.

Pensez à un `Store` comme à une immense base de données ultra-rapide et organisée, dédiée aux données de jeu. Chaque `Store` est spécifique à un type d'environnement (`ECS_TYPE`), par exemple, un `Store<EntityStore>` gérera les entités dans un monde de jeu.

---

### 💡 Le Concept du "Store" en un coup d'œil

Un `Store` gère :
-   **Les Entités :** Chaque entité est un simple identifiant (`Ref<ECS_TYPE>`).
-   **Les Composants :** Des petites "boîtes de données" attachées aux entités.
-   **Les Archétypes :** Des "modèles" qui décrivent quelles combinaisons de composants une entité possède.
-   **La Thread-Safety :** Il s'assure que plusieurs parties du jeu ne modifient pas les mêmes données en même temps, ce qui pourrait causer des erreurs.

---

### ⚙️ Comment interagir avec un Store (les méthodes clés)

La classe `Store` n'est généralement pas instanciée directement par les développeurs de mods. Vous recevrez souvent une instance de `Store` dans des méthodes de systèmes ou d'événements (comme dans `InteractiveCustomUIPage.handleDataEvent()`).

#### Récupérer un Composant (la méthode la plus courante)

C'est la méthode que vous utiliserez le plus souvent. Elle permet d'obtenir un composant spécifique attaché à une entité.

```java
public <T extends Component<ECS_TYPE>> T getComponent( @Nonnull Ref<ECS_TYPE> ref, @Nonnull ComponentType<ECS_TYPE, T> componentType)
```

-   **`ref`** : La référence (`Ref`) à l'entité qui possède le composant.
-   **`componentType`** : Le type du composant que vous voulez récupérer (par exemple, `Player.getComponentType()` pour le composant `Player`).
-   **Retourne :** L'instance du composant si l'entité le possède, sinon `null`.

**Exemple :**
```java
// Imaginons que 'ref' est la Ref d'un joueur, et 'store' le Store actuel.
// Nous voulons récupérer le composant 'Player' de ce joueur.
Player playerComponent = store.getComponent(ref, Player.getComponentType());

if (playerComponent != null) {
    // Le joueur a bien un composant Player, on peut l'utiliser
    playerComponent.getPageManager().openCustomPage(...);
}
```

#### Ajouter un Composant

Permet d'ajouter un nouveau composant à une entité. Si l'entité possédait déjà un composant de ce type, il sera remplacé.

```java
public <T extends Component<ECS_TYPE>> void addComponent( @Nonnull Ref<ECS_TYPE> ref, @Nonnull ComponentType<ECS_TYPE, T> componentType, @Nonnull T component)
```

#### Supprimer un Composant

Retire un composant d'une entité.

```java
public <T extends Component<ECS_TYPE>> void removeComponent( @Nonnull Ref<ECS_TYPE> ref, @Nonnull ComponentType<ECS_TYPE, T> componentType)
```

#### `public int getEntityCount()`

Retourne le nombre total d'entités actuellement gérées par ce `Store`.

#### `public ECS_TYPE getExternalData()`

C'est une méthode très utile ! Pour un `Store<EntityStore>`, `getExternalData()` retournera l'`EntityStore` lui-même, qui contient souvent un accès au `World` (monde) associé.

**Exemple :**
```java
World world = store.getExternalData().getWorld();
```

---

### 🔒 La Thread-Safety : Pourquoi c'est important

La classe `Store` est conçue pour fonctionner dans un environnement où plusieurs "threads" (des séquences d'exécution de code) peuvent essayer de lire ou de modifier des données en même temps. Pour éviter que cela ne cause des erreurs bizarres (appelées "race conditions" ou "corruption de données"), le `Store` utilise des mécanismes de verrouillage.

#### `public void assertThread()`
Cette méthode vérifie que le code qui appelle une opération sur le `Store` est bien exécuté sur le **bon thread** (généralement le thread principal du monde ou un thread spécifiquement désigné). Si ce n'est pas le cas, elle lance une `IllegalStateException`. C'est pourquoi nous utilisons `world.execute(() -> { ... })` pour garantir que nos actions sur l'UI (qui affectent les entités) sont exécutées sur le thread du monde.

#### `public void assertWriteProcessing()`
Cette méthode s'assure qu'une modification (`write`) n'est pas effectuée pendant que le `Store` est en train de traiter d'autres opérations complexes. C'est une autre couche de protection pour maintenir la cohérence des données.

---

### ⚠️ Point important : `Ref<ECS_TYPE>` vs `Holder<ECS_TYPE>`

-   **`Ref<ECS_TYPE>` (Référence)** : C'est un simple identifiant numérique qui pointe vers une entité active dans le `Store`. C'est léger et efficace pour référencer une entité.
-   **`Holder<ECS_TYPE>` (Conteneur Temporaire)** : C'est une "copie" temporaire d'une entité et de ses composants, utilisée souvent lors de l'ajout ou du retrait d'entités du `Store`, ou lorsque des modifications importantes sont en cours. C'est comme une zone de transit sécurisée pour les données d'entité.

---

### 📚 Pour aller plus loin : ECS et le `Store`

Le `Store` est l'implémentation concrète de la partie "S" (Système) et "E" (Entité) dans le pattern ECS, en gérant le stockage des "C" (Composants). Comprendre comment il fonctionne est clé pour écrire des mods performants et stables.
