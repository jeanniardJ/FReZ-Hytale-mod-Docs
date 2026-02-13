# ⚙️ Configuration & Codecs

Hytale utilise un système puissant de `Codec` pour mapper automatiquement les fichiers JSON vers des objets Java.

## 1. Structure du Fichier JSON (`config.json`)
```json
{
  "Welcome": {
    "Message": "Bienvenue !"
  },
  "Settings": {
    "MaxPlayers": 100,
    "Motd": ["Ligne 1", "Ligne 2"]
  }
}
```

## 2. Classe Java Correspondante
Il faut utiliser `BuilderCodec` pour construire le mapping champ par champ.

### `KeyedCodec` et les Règles de Nommage

Chaque champ dans le JSON est mappé via un `KeyedCodec`. C'est une enveloppe qui associe une `clé` (le nom du champ dans le JSON) à un `Codec` (qui sait comment lire/écrire la valeur).

**Règles de nommage pour les clés (très important !) :**
1.  La clé ne doit pas être vide.
2.  **Si la clé commence par une lettre, elle doit être une majuscule.**
    *   ✅ `new KeyedCodec<>("MaxPlayers", ...)` -> Valide.
    *   ❌ `new KeyedCodec<>("maxPlayers", ...)` -> **Invalide**, lèvera une `IllegalArgumentException`.
3.  **Si la clé commence par un symbole (comme `@`)**, la vérification de la majuscule est ignorée.
    *   ✅ `new KeyedCodec<>("@InputValue", ...)` -> **Valide**. C'est une convention cruciale pour la liaison de données dans les interfaces utilisateur, où le `@` indique au système de récupérer la *valeur* d'un champ UI.

### Exemple de `BuilderCodec`

```java
public class MyConfig {
    // Champs de données
    public WelcomeConfig welcome = new WelcomeConfig();
    public SettingsConfig settings = new SettingsConfig();

    // LE CODEC PRINCIPAL
    public static final BuilderCodec<MyConfig> CODEC = BuilderCodec.builder(MyConfig.class, MyConfig::new)
            .append(new KeyedCodec<>("Welcome", WelcomeConfig.CODEC), (c, v, i) -> c.welcome = v, (c, i) -> c.welcome)
            .append(new KeyedCodec<>("Settings", SettingsConfig.CODEC), (c, v, i) -> c.settings = v, (c, i) -> c.settings)
            .build();

    // SOUS-CLASSE (Welcome)
    public static class WelcomeConfig {
        public String message = "Default";
        
        public static final Codec<WelcomeConfig> CODEC = BuilderCodec.builder(WelcomeConfig.class, WelcomeConfig::new)
                .append(new KeyedCodec<>("Message", Codec.STRING), (c, v, i) -> c.message = v, (c, i) -> c.message)
                .build();
    }
    
    // SOUS-CLASSE (Settings)
    public static class SettingsConfig {
        public int maxPlayers = 50;
        public String[] motd = {};

        public static final Codec<SettingsConfig> CODEC = BuilderCodec.builder(SettingsConfig.class, SettingsConfig::new)
                .append(new KeyedCodec<>("MaxPlayers", Codec.INTEGER), (c, v, i) -> c.maxPlayers = v, (c, i) -> c.maxPlayers)
                .append(new KeyedCodec<>("Motd", Codec.STRING_ARRAY), (c, v, i) -> c.motd = v, (c, i) -> c.motd)
                .build();
    }
}
```

## 3. Chargement dans le Plugin
```java
// 1. Enregistrement (Constructeur)
this.config = withConfig("config", MyConfig.CODEC);

// 2. Lecture (start)
MyConfig data = config.get();
System.out.println(data.welcome.message);
```

---

## 4. Liste des Codecs de Base
Voici les codecs standards disponibles dans `com.hypixel.hytale.codec.Codec`.

| Codec | Type Java | Exemple JSON |
| :--- | :--- | :--- |
| `Codec.STRING` | `String` | `"texte"` |
| `Codec.BOOLEAN` | `boolean` | `true` |
| `Codec.INTEGER` | `int` | `123` |
| `Codec.LONG` | `long` | `1234567890` |
| `Codec.FLOAT` | `float` | `1.23` |
| `Codec.DOUBLE` | `double` | `1.23456` |
| `Codec.BYTE` | `byte` | `12` |
| `Codec.SHORT` | `short` | `123` |
| | | |
| `Codec.STRING_ARRAY` | `String[]` | `["a", "b"]` |
| `Codec.INT_ARRAY` | `int[]` | `[1, 2, 3]` |
| `Codec.LONG_ARRAY` | `long[]` | `[1, 2, 3]` |
| `Codec.FLOAT_ARRAY` | `float[]` | `[1.1, 2.2]` |
| `Codec.DOUBLE_ARRAY` | `double[]` | `[1.1, 2.2]` |
| | | |
| `Codec.PATH` | `java.nio.file.Path` | `"C:/path/to/file"` |
| `Codec.UUID_STRING` | `java.util.UUID` | `"uuid-string"` |
| `Codec.INSTANT` | `java.time.Instant` | `"2024-01-01T12:00:00Z"` |
| `Codec.DURATION` | `java.time.Duration` | `"PT15M"` (15 minutes) |
| `Codec.DURATION_SECONDS` | `java.time.Duration` | `900.0` (15 minutes) |
| `Codec.LOG_LEVEL` | `java.util.logging.Level` | `"INFO"` |
