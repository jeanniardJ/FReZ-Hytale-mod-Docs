# Événement : PlayerConnectEvent

Cet événement est l'un des plus importants pour un développeur de plugins. Il vous permet de savoir **exactement quand un joueur se connecte** à votre serveur.

---

### À quoi ça sert concrètement ?

Dès qu'un joueur rejoint le jeu, vous pouvez déclencher une action. Voici quelques exemples :
- Lui souhaiter la bienvenue dans le chat.
- Charger ses données depuis une base de données (son argent, ses permissions, etc.).
- Le téléporter à un point de spawn précis.
- Lui donner des objets de départ.

### Quand est-il déclenché ?

Cet événement est lancé par le serveur juste après qu'un joueur a réussi à se connecter, mais **avant qu'il n'apparaisse physiquement dans le monde**. C'est le moment idéal pour préparer tout ce qui le concerne.

---

### ✅ Exemple d'utilisation (La bonne méthode)

Voici comment écouter cet événement dans votre plugin.

1.  **Créez une méthode dans une classe "Listener" :**
    ```java
    public class MyPlayerListener {
        public void onPlayerJoin(PlayerConnectEvent event) {
            // On utilise getPlayerRef(), c'est la méthode moderne et sûre.
            PlayerRef playerRef = event.getPlayerRef();
            UUID playerUuid = playerRef.getUuid();
            String playerName = playerRef.getUsername();

            HytaleLogger.info("Le joueur " + playerName + " (" + playerUuid + ") vient de se connecter !");

            // Envoyer un message de bienvenue privé
            playerRef.sendMessage(Message.raw("Bienvenue sur le serveur, " + playerName + " !"));
        }
    }
    ```

2.  **Enregistrez votre listener dans la méthode `start()` de votre plugin :**
    ```java
    @Override
    public void start() {
        MyPlayerListener listener = new MyPlayerListener();

        // On dit à Hytale : "Quand un PlayerConnectEvent se produit, appelle la méthode onPlayerJoin de mon listener"
        hytaleServer.getEventBus().register(PlayerConnectEvent.class, listener::onPlayerJoin);
    }
    ```

---

### 📖 Concepts Clés & Glossaire

Le code de cet événement utilise des termes techniques. Voici ce qu'ils signifient :

| Terme | Explication Simple |
| :--- | :--- |
| **`PlayerRef`** | C'est une **référence** légère à un joueur connecté. Pensez-y comme à un "raccourci" vers le joueur. C'est l'objet que vous devez utiliser 99% du temps pour interagir avec un joueur (lui envoyer un message, obtenir son UUID, etc.). |
| **`Player` (Component)** | C'est le **composant** qui contient les *données de jeu* du joueur (sa vie, son inventaire, etc.). On y accède plus rarement, seulement quand on a besoin de modifier son état en jeu. |
| **`@Deprecated`** | C'est une étiquette que les développeurs mettent sur du code (une méthode, une classe) pour dire : **"N'utilisez plus ceci !"**. Ce code fonctionne encore, mais il est vieux, et il sera supprimé dans une future mise à jour. Utiliser du code déprécié est une mauvaise pratique. |
| **`Holder<EntityStore>`** | Imaginez une "boîte de transport" temporaire. Quand un joueur se connecte, ses données (composants) sont mises dans ce `Holder` avant d'être officiellement placées dans le monde du jeu. C'est un conteneur transitoire. |
| **`EntityStore`** | C'est la grande "base de données" en mémoire qui contient toutes les entités et leurs composants. |

---

### ⚙️ Détail des Méthodes de `PlayerConnectEvent`

#### `public PlayerRef getPlayerRef()`
C'est **LA méthode à utiliser** pour obtenir le joueur. Elle retourne une référence `PlayerRef` fiable et moderne.

#### `public Holder<EntityStore> getHolder()`
Retourne le conteneur temporaire (`Holder`) avec les composants du joueur. Utile dans des cas avancés si vous avez besoin d'accéder aux composants avant que le joueur ne soit pleinement dans le monde.

#### `public World getWorld()`
Retourne le monde (`World`) dans lequel le joueur va apparaître.

#### `public void setWorld(World world)`
Permet de changer le monde dans lequel le joueur va apparaître. Utile pour rediriger un joueur vers un monde spécifique (ex: un lobby) dès sa connexion.

#### `public Player getPlayer()`
<br>
<div style="background-color: #4B0000; color: white; padding: 15px; border-left: 5px solid #FF5555;">
    <h3>⚠️ Méthode Dépréciée - À ne pas utiliser !</h3>
    <p>
        Cette méthode est marquée <strong><code>@Deprecated</code></strong>. Elle est conservée pour la compatibilité avec d'ancien code, mais elle est destinée à être supprimée.
    </p>
    <p>
        <strong>Raison :</strong> Elle retourne l'ancien objet `Player`, qui est moins flexible que le système moderne basé sur `PlayerRef` et les composants. Utiliser cette méthode peut rendre votre code instable dans les futures mises à jour de Hytale.
    </p>
    <p>
        <strong>Toujours utiliser <code>event.getPlayerRef()</code> à la place.</strong>
    </p>
</div>

---

### 🔍 Guide de Débogage

**"Mon listener ne se déclenche pas, rien ne s'affiche dans la console."**

1.  **Vérifiez l'enregistrement :** Avez-vous bien ajouté la ligne `hytaleServer.getEventBus().register(...)` dans la méthode `start()` de votre plugin principal ? C'est l'erreur la plus fréquente.
2.  **Vérifiez la signature de la méthode :** Votre méthode doit être `public` et accepter un seul paramètre de type `PlayerConnectEvent`.
3.  **Vérifiez les logs de démarrage :** Le serveur affiche-t-il une erreur liée à votre plugin au démarrage ? Si oui, lisez-la attentivement.
