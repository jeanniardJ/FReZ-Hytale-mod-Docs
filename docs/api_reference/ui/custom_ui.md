# 🖥️ Custom UI (Interfaces Utilisateur)

Hytale permet de créer des interfaces graphiques interactives (GUI) pour les joueurs.

## 1. Créer une Page (`InteractiveCustomUIPage`)

Une page UI est une classe Java qui hérite de `InteractiveCustomUIPage`. Elle définit la structure de l'interface et
gère les événements.

```java
public class MyPage extends InteractiveCustomUIPage<MyPage.MyEventData> {

    // Définition de la structure des données d'événement (Codec)
    public static class MyEventData {
        public String action;
        public String inputValue;

        public static final Codec<MyEventData> CODEC = BuilderCodec.builder(MyEventData.class, MyEventData::new)
                .append(new KeyedCodec<>("Action", Codec.STRING), (o, v) -> o.action = v, o -> o.action)
                // Pour binder un input, la clé doit commencer par '@' dans le Codec ET dans l'EventData
                .append(new KeyedCodec<>("@InputValue", Codec.STRING), (o, v) -> o.inputValue = v, o -> o.inputValue)
                .build();
    }

    public MyPage(PlayerRef player) {
        super(player, CustomPageLifetime.CanDismiss, MyEventData.CODEC);
    }

    @Override
    public void build(Ref<EntityStore> ref, UICommandBuilder cmd, UIEventBuilder events, Store<EntityStore> store) {
        // Charger un fichier template .ui
        cmd.append("Pages/MyPage.ui");

        // Modifier un élément statique
        cmd.set("#Title.Text", "Mon Titre");

        // Binding d'un bouton simple
        EventData btnEvent = new EventData();
        btnEvent.append("Action", "CLICK_BUTTON");
        events.addEventBinding(CustomUIEventBindingType.Activating, "#MyButton", btnEvent);

        // Binding d'un bouton avec récupération de valeur (Input)
        EventData saveEvent = new EventData();
        saveEvent.append("Action", "SAVE");
        // '@' indique qu'on veut la valeur de l'élément UI, pas la chaîne littérale
        saveEvent.append("@InputValue", "#MyInput.Value");
        events.addEventBinding(CustomUIEventBindingType.Activating, "#SaveButton", saveEvent);
    }

    @Override
    public void handleDataEvent(Ref<EntityStore> ref, Store<EntityStore> store, MyEventData data) {
        if ("SAVE".equals(data.action)) {
            // data.inputValue contient le texte saisi par le joueur
            Log.info("Valeur reçue : " + data.inputValue);
        } else if ("CLOSE".equals(data.action)) {
            // Fermer la page
            this.close();
        }
    }
}
```

## 2. Listes Dynamiques

Pour afficher une liste d'éléments variable, il faut utiliser des sélecteurs indexés car `cmd.append` ne permet pas de
définir un ID unique à la volée.

```java
List<String> items = List.of("Item 1", "Item 2", "Item 3");
int index = 0;

for (String item : items) {
    // 1. Ajouter le template de l'item à la liste parente
    cmd.append("#MyListContainer", "Pages/ItemTemplate.ui");
    
    // 2. Cibler le N-ième enfant de la liste
    String selector = "#MyListContainer[" + index + "]";
    
    // 3. Modifier les propriétés via ce sélecteur relatif
    cmd.set(selector + " #ItemLabel.Text", item);
    
    // 4. Ajouter des événements spécifiques
    EventData event = new EventData();
    event.append("Action", "SELECT_ITEM");
    event.append("Index", String.valueOf(index));
    events.addEventBinding(CustomUIEventBindingType.Activating, selector + " #SelectBtn", event);
    
    index++;
}
```

## 3. Ouvrir l'Interface

L'ouverture doit se faire sur le thread du monde pour éviter les problèmes de concurrence.

```java
// Récupérer le monde depuis le store du joueur
World world = store.getExternalData().getWorld();

world.execute(() -> {
    PlayerRef playerRef = ...; // Récupérer le PlayerRef
    Player playerComponent = ...; // Récupérer le composant Player
    
    // Ouvrir la page
    playerComponent.getPageManager().openCustomPage(ref, store, new MyPage(playerRef));
});
```

## 4. Style des Boutons

Vous pouvez modifier la couleur des boutons directement via le code :

```java
// Couleur de fond
cmd.set("#MyButton.Color", "#22c55e"); // Vert

// Couleur au survol
cmd.set("#MyButton.HoverColor", "#4ade80"); // Vert clair
```

## 5. Structure des Fichiers UI

Les fichiers `.ui` utilisent un format proche du JSON/HJSON.

**Attention :** L'utilisation de variables globales comme `$C = "../Common.ui";` peut causer des erreurs si le fichier
référencé n'existe pas. Il est recommandé de commenter ces lignes si vous n'avez pas le fichier commun.

```hjson
Group {
  LayoutMode: Center;
  // ...
}
```

## 6. Dépannage (Troubleshooting)

### "Failed to load CustomUI documents"

**Cause** : Erreur de syntaxe dans votre fichier `.ui`.
**Solutions** :

1. Vérifiez que toutes les valeurs textuelles sont entre guillemets : `Text: "Hello";` (pas `Text: Hello;`).
2. Vérifiez que toutes les propriétés se terminent par un point-virgule `;`.
3. Vérifiez le format des couleurs : `#ffffff` ou `#fff`.
4. Assurez-vous que l'import de `Common.ui` est correct ou commenté si le fichier n'existe pas : `$C = "../Common.ui";`.

### "Failed to apply CustomUI event bindings"

**Cause** : L'ID de l'élément dans le code Java ne correspond pas à celui du fichier `.ui`.
**Solutions** :

1. Vérifiez que l'ID existe dans le fichier `.ui`.
2. Vérifiez l'orthographe : `#MyButton` en Java doit correspondre à `#MyButton` dans le `.ui`.

### "Selected element in CustomUI command was not found"

**Cause** : Sélecteur incorrect pour les templates ajoutés dynamiquement.
**Compréhension** : Quand vous ajoutez un template à un conteneur, le template lui-même **devient** l'élément à cet
index.
**Correct** : `cmd.set("#Container[0].Text", "Hello");`
**Incorrect** : `cmd.set("#Container[0] #Button.Text", "Hello");` (sauf si le template a des IDs imbriqués).

### Déconnexion du joueur à l'ouverture de la page

**Cause** : Le fichier `.ui` a une erreur de parsing ou n'existe pas.
**Solutions** :

1. Vérifiez que le chemin du fichier correspond : `"YourPlugin/MyPage.ui"` correspond à
   `Common/UI/Custom/YourPlugin/MyPage.ui`.
2. Revoyez la syntaxe du fichier UI.

## 7. Exemple Concret : La Page des Souvenirs (MemoriesPage)

Pour illustrer ces concepts, analysons `MemoriesPage.java`, une classe complexe qui gère l'interface des "Souvenirs" dans le jeu.

### Objectif de la Page

Cette page permet au joueur de visualiser les souvenirs qu'il a collectés, organisés par catégories. Elle affiche la progression totale et les détails de chaque souvenir.

### Structure de la Classe

La classe `MemoriesPage` hérite de `InteractiveCustomUIPage` et gère deux affichages principaux :
1.  **La vue des catégories** : Affiche une liste de toutes les catégories de souvenirs.
2.  **La vue des souvenirs** : Affiche les souvenirs d'une catégorie sélectionnée.

Un état interne, `currentCategory`, permet de savoir quelle vue afficher.

### `build()` - Construction de l'Interface

La méthode `build()` est le cœur de la logique d'affichage. Voici comment elle fonctionne :

1.  **Si `currentCategory` est `null`**, on affiche la liste des catégories :
    *   On charge un template de base : `cmd.append("Pages/Memories/MemoriesCategoryPanel.ui");`
    *   On récupère les données (tous les souvenirs, ceux déjà enregistrés) via `MemoriesPlugin.get()`.
    *   On calcule et affiche la barre de progression.
    *   On boucle sur chaque catégorie pour l'ajouter à une liste dynamique (`#IconList`), en utilisant la technique du sélecteur indexé.
    *   Pour chaque catégorie, on affiche si elle est complétée et on ajoute un indicateur si de nouveaux souvenirs y ont été ajoutés.
    *   On lie un événement au clic sur chaque catégorie pour passer à la vue des souvenirs (`PageAction.ViewCategory`).

2.  **Si `currentCategory` a une valeur**, on affiche les souvenirs de cette catégorie :
    *   On charge un autre template : `cmd.append("Pages/Memouries/MemoriesPanel.ui");`
    *   On trie les souvenirs par ordre alphabétique.
    *   On boucle sur la liste des souvenirs pour les afficher, en marquant ceux qui sont déjà découverts.
    *   Si un souvenir est sélectionné (`selectedMemory`), on affiche ses détails (nom, date, lieu).
    *   On ajoute un bouton "Retour" (`PageAction.Back`) pour revenir à la vue des catégories.

### `handleDataEvent()` - Gestion des Actions

Cette méthode reçoit les événements déclenchés par le joueur dans l'interface, encapsulés dans l'objet `PageEventData`.

*   **`PageAction.ViewCategory`**: Met à jour `this.currentCategory` avec la catégorie choisie et appelle `this.rebuild()` pour redessiner l'interface.
*   **`PageAction.Back`**: Remet `this.currentCategory` à `null` et appelle `this.rebuild()` pour afficher à nouveau les catégories.
*   **`PageAction.SelectMemory`**: Met à jour `this.selectedMemory`, puis met à jour l'affichage des détails du souvenir sans tout redessiner, via un `UICommandBuilder` envoyé directement.

### `PageEventData` et `PageAction` - Communication UI ↔ Serveur

*   La classe interne `PageEventData` définit les données qui transitent entre le client (l'interface) et le serveur (le code Java). Son `CODEC` est crucial pour sérialiser/désérialiser ces données.
*   L'énumération `PageAction` standardise les types d'actions possibles, ce qui rend le `switch` dans `handleDataEvent()` clair et robuste.

Cet exemple montre comment une page UI complexe peut être gérée avec un état interne, plusieurs templates, et une logique de construction dynamique pour créer une expérience riche pour le joueur.

### `MemoriesUnlockedPage` - Une Page de Transition

Cette seconde page, beaucoup plus simple, sert d'écran de transition.

*   **`build()`** : Elle charge un unique template (`Pages/Memories/MemoriesUnlocked.ui`) et ajoute un seul bouton, `#DiscoverMemoriesButton`.
*   **`handleDataEvent()`** : Lorsque le joueur clique sur le bouton, l'événement `DiscoverMemories` est reçu. La seule action est alors d'ouvrir la page principale `MemoriesPage`.

C'est un bon exemple de cómo les pages peuvent s'enchaîner pour créer un flux de navigation pour l'utilisateur.
