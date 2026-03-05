# 🧱 BlockEntity API

Ce document explique ce qu'est un `BlockEntity`, comment il s'intègre dans l'architecture ECS de Hytale, et comment l'utiliser dans vos propres systèmes. Ce guide est conçu pour un développeur junior.

## 1. Qu'est-ce qu'un BlockEntity ?

### Définitions de base

*   **Entité :** Dans une architecture [ECS (Entité-Composant-Système)](../../1_ARCHITECTURE.md), une **entité** n'est qu'un identifiant unique. C'est comme une coquille vide. Un joueur, un monstre, ou même un objet au sol sont des entités.
*   **Composant (Component) :** Un **composant** est un bloc de données qui est attaché à une entité. Il n'a pas de logique (pas de `update()`, par exemple). Il contient juste des informations. Exemples : `HealthComponent` (données de vie), `PositionComponent` (coordonnées x, y, z).
*   **Système (System) :** Un **système** contient la logique. Il opère sur des entités qui possèdent un certain ensemble de composants. Par exemple, un `PhysicsSystem` pourrait agir sur toutes les entités qui ont un `PositionComponent` et un `VelocityComponent`.

### Le `BlockEntity`

Un `BlockEntity` est un **composant** spécial. Il représente un bloc du monde qui a été transformé en une entité dynamique. Pensez à un bloc de sable qui commence à tomber : il n'est plus statique dans la grille du monde, il devient une entité avec une position, une vitesse, et des propriétés physiques.

Le `BlockEntity` est le composant qui dit : "Cette entité se comporte et ressemble à un bloc de tel ou tel type".

---

## 2. Cas d'usage concrets

Vous utiliserez un `BlockEntity` dans des situations comme :

*   **Blocs qui tombent :** Quand un bloc de sable ou de gravier n'a plus de support, le jeu le détruit et crée une entité à sa place, lui attache un `BlockEntity` (pour savoir que c'est du sable) et des composants de physique pour le faire tomber.
*   **Blocs lancés par des pistons :** Un piston qui pousse un bloc peut le transformer temporairement en entité pour gérer son mouvement.
*   **Effets visuels :** Créer des blocs "fantômes" ou des animations de blocs qui se construisent.

---

## 3. Créer un BlockEntity

La manière la plus simple de créer un `BlockEntity` est d'utiliser la méthode statique `assembleDefaultBlockEntity`. Elle prépare une nouvelle entité avec tous les composants de base nécessaires.

### Exemple de code : Faire apparaître un bloc de pierre qui tombe

```java
import com.hypixel.hytale.server.core.entity.entities.BlockEntity;
import com.hypixel.hytale.server.core.universe.world.storage.EntityStore;
import com.hypixel.hytale.component.Holder;
import com.hypixel.hytale.math.vector.Vector3d;
import com.hypixel.hytale.server.core.modules.time.TimeResource;
import com.hypixel.hytale.server.api.HytaleServer; // Pour l'exemple, obtenir TimeResource

// Contexte : à l'intérieur d'un système ou d'un listener d'événement.

// 1. Obtenir la ressource de temps du serveur.
// TimeResource est nécessaire pour gérer le cycle de vie de l'entité (ex: sa disparition).
TimeResource time = HytaleServer.getInstance().getWorlds().get(0).getUniverse(). getResource(TimeResource.class);

// 2. Définir la position de départ.
// Créons notre bloc à la position x=10, y=50, z=20.
Vector3d startPosition = new Vector3d(10, 50, 20);

// 3. Définir le type de bloc.
// La "blockTypeKey" est une chaîne qui identifie le bloc.
// Vous pouvez trouver ces clés dans les fichiers d'assets du jeu.
String stoneBlockKey = "hytale:stone";

// 4. Assembler l'entité.
// Cette méthode crée une nouvelle entité et y attache :
// - Un BlockEntity (avec la clé "hytale:stone")
// - Un TransformComponent (pour la position)
// - Un DespawnComponent (pour qu'il disparaisse après 120s par défaut)
// - Un VelocityComponent (pour la vitesse)
// - Un UUIDComponent (un identifiant unique)
Holder<EntityStore> blockEntityHolder = BlockEntity.assembleDefaultBlockEntity(time, stoneBlockKey, startPosition);

// 5. (Optionnel) Ajouter une force initiale.
// Le bloc va maintenant tomber grâce à la gravité gérée par le système de physique.
// Mais on peut aussi lui donner une poussée.
BlockEntity blockComponent = blockEntityHolder.getComponent(BlockEntity.getComponentType());
if (blockComponent != null) {
    // Ajoute une force vers le haut et vers l'avant.
    blockComponent.addForce(0, 5.0f, 2.0f);
}

// L'entité est maintenant créée dans le monde et la physique s'appliquera.
```

---

## 4. Propriétés et Méthodes Principales

Voici les méthodes les plus utiles du composant `BlockEntity`.

### `getBlockTypeKey()`
Récupère la chaîne de caractères qui identifie le type de bloc.

```java
String type = blockComponent.getBlockTypeKey(); // Retourne "hytale:stone"
```

### `setBlockTypeKey(String newKey, ...)`
Change le type de bloc de l'entité en plein vol.

```java
// Contexte : nécessite une référence à l'entité (ref) et un CommandBuffer.
// Un CommandBuffer est utilisé pour enregistrer des changements sur les composants
// de manière sécurisée à travers les threads.

Ref<EntityStore> entityRef = blockEntityHolder.getRef();
CommandBuffer<EntityStore> commandBuffer = ...; // Obtenu depuis un système

blockComponent.setBlockTypeKey("hytale:dirt", entityRef, commandBuffer);
// L'entité ressemble maintenant à un bloc de terre.
```

### `addForce(float x, float y, float z)`
Applique une force à l'entité, modifiant sa vélocité. Utile pour les explosions ou les interactions.

```java
// Pousse le bloc vers le haut.
blockComponent.addForce(0, 10.0f, 0);
```

---

## 5. Bonnes Pratiques et Intégration

### Utiliser le Logger Hytale
Pour déboguer, évitez `System.out.println`. Utilisez le système de log de Hytale, qui est plus propre et configurable.

```java
import com.hypixel.hytale.server.api.HytaleServer;
import org.apache.logging.log4j.Logger;

private static final Logger LOGGER = HytaleServer.getLogger();

LOGGER.info("Création d'un BlockEntity de type: {}", stoneBlockKey);
```

### Intégration avec les Événements (Events)
Votre logique de création de `BlockEntity` sera souvent déclenchée par un événement. Par exemple, vous pourriez écouter un `BlockBreakEvent` et, sous certaines conditions, annuler l'événement et créer un `BlockEntity` à la place.

### Liens avec la vision du projet
*   **Architecture :** Pour comprendre comment ce composant s'inscrit dans la vision globale de nos mods, consultez le document sur [l'architecture FReZ Hytale](file:///C:\Users\jonat\Documents\Hytal\Pluginsesume_architecture_frez_hytale.md).
*   **Stories :** Pour voir des exemples de comment les fonctionnalités sont planifiées et découpées, référez-vous à nos [user stories](file:///C:\Users\jonat\Documents\Hytal\Plugins\FReZ-Hytale-mod-CorePlayer\stories.md).
