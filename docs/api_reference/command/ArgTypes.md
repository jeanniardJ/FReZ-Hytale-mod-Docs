# ArgTypes

`com.hypixel.hytale.server.core.command.system.arguments.types.ArgTypes`

Une classe utilitaire contenant les types d'arguments standard pour l'analyse des commandes.

## Types Standard

| Champ | Type | Description | Exemples |
|-------|------|-------------|----------|
| `BOOLEAN` | `Boolean` | Valeurs booléennes | `true`, `false` |
| `INTEGER` | `Integer` | Nombres entiers | `-1`, `0`, `1`, `56346` |
| `STRING` | `String` | Chaînes de texte | `"Bonjour"`, `"Texte"` |
| `FLOAT` | `Float` | Nombres à virgule flottante | `3.14`, `-2.5` |
| `DOUBLE` | `Double` | Nombres à double précision | `-3.14`, `0.0` |
| `UUID` | `UUID` | Chaînes UUID | `550e8400-e29b-41d4-a716-446655440000` |

## Types Joueur & Entité

| Champ | Type | Description |
|-------|------|-------------|
| `PLAYER_UUID` | `UUID` | Résout un nom de joueur ou une chaîne UUID en UUID. Vérifie d'abord les joueurs en ligne. |
| `GAME_PROFILE_LOOKUP` | `PublicGameProfile` | Recherche synchrone d'un profil de jeu par nom ou UUID. |
| `GAME_PROFILE_LOOKUP_ASYNC` | `CompletableFuture<PublicGameProfile>` | Recherche asynchrone d'un profil de jeu. |
| `PLAYER_REF` | `PlayerRef` | Résout un nom d'utilisateur de joueur en `PlayerRef`. |
| `ENTITY_ID` | `UUID` | Argument encapsulé pour l'ID d'entité. |

## Types Coordonnées & Position

| Champ | Type | Description | Exemples |
|-------|------|-------------|----------|
| `RELATIVE_DOUBLE_COORD` | `Coord` | Coordonnée pouvant être relative (`~`). | `5.0`, `~-2.3` |
| `RELATIVE_INT_COORD` | `IntCoord` | Coordonnée entière pouvant être relative (`~`). | `5`, `~-2` |
| `RELATIVE_INTEGER` | `RelativeInteger` | Entier pouvant être relatif. | `5`, `~-2` |
| `RELATIVE_FLOAT` | `RelativeFloat` | Flottant pouvant être relatif. | `90.0`, `~` |
| `VECTOR2I` | `Vector2i` | Vecteur entier 2D (x, z). | `124 232` |
| `VECTOR3I` | `Vector3i` | Vecteur entier 3D (x, y, z). | `124 232 234` |
| `RELATIVE_VECTOR3I` | `RelativeVector3i` | Vecteur entier relatif 3D. | `~5 0 ~-3` |
| `RELATIVE_BLOCK_POSITION` | `RelativeIntPosition` | Position de bloc relative (x, y, z). | `~5 ~ ~-3` |
| `RELATIVE_POSITION` | `RelativeDoublePosition` | Position précise relative (x, y, z). | `~5.5 ~ ~` |
| `RELATIVE_CHUNK_POSITION` | `RelativeChunkPosition` | Coordonnées de chunk relatives (x, z). | `5 10`, `~ ~` |
| `ROTATION` | `Vector3f` | Vecteur de rotation (pitch, yaw, roll). | `0 90 0` |

## Types d'Assets

Ces types résolvent vers des assets spécifiques du jeu.

* `MODEL_ASSET`: `ModelAsset`
* `WEATHER_ASSET`: `Weather`
* `INTERACTION_ASSET`: `Interaction`
* `ROOT_INTERACTION_ASSET`: `RootInteraction`
* `EFFECT_ASSET`: `EntityEffect`
* `ENVIRONMENT_ASSET`: `Environment`
* `ITEM_ASSET`: `Item`
* `BLOCK_TYPE_ASSET`: `BlockType`
* `PARTICLE_SYSTEM`: `ParticleSystem`
* `HITBOX_COLLISION_CONFIG`: `HitboxCollisionConfig`
* `REPULSION_CONFIG`: `RepulsionConfig`
* `SOUND_EVENT_ASSET`: `SoundEvent`
* `AMBIENCE_FX_ASSET`: `AmbienceFX`

## Types Bloc & Monde

| Champ | Type | Description |
|-------|------|-------------|
| `WORLD` | `World` | Résout un nom de monde en objet `World`. |
| `BLOCK_TYPE_KEY` | `String` | Une chaîne identifiant un type de bloc. |
| `BLOCK_ID` | `Integer` | Résout une clé de type de bloc en son ID entier. |
| `WEIGHTED_BLOCK_TYPE` | `Pair<Integer, String>` | Un type de bloc avec un poids associé. |
| `BLOCK_PATTERN` | `BlockPattern` | Un motif de blocs avec des poids. |
| `BLOCK_MASK` | `BlockMask` | Un masque pour filtrer les blocs (ex: `>Grass_Full`, `!Fluid_Water`). |

## Types Divers

| Champ | Type | Description |
|-------|------|-------------|
| `COLOR` | `Integer` | Analyse les couleurs hexadécimales (`#RRGGBB`, `0x...`) ou les entiers. |
| `TICK_RATE` | `Integer` | Analyse les taux de tick ou les durées (`30tps`, `50ms`). |
| `GAME_MODE` | `GameMode` | Sélection du mode de jeu. |
| `SOUND_CATEGORY` | `SoundCategory` | Enumération des catégories de sons. |
| `INT_RANGE` | `Pair<Integer, Integer>` | Plage de deux entiers (min max). |
| `RELATIVE_INT_RANGE` | `RelativeIntegerRange` | Plage de deux entiers relatifs. |
| `INTEGER_COMPARISON_OPERATOR` | `IntegerComparisonOperator` | Opérateurs: `>`, `<`, `>=`, `<=`, `%`, `=`, `!=` |
| `INTEGER_OPERATION` | `IntegerOperation` | Opérateurs: `+`, `-`, `*`, `/`, `%`, `=` |

## Méthodes Utilitaires

### forEnum

```java
public static <E extends Enum<E>> SingleArgumentType<E> forEnum(String name, @Nonnull Class<E> enumType)
```
Crée un type d'argument pour une classe Enum spécifique.
