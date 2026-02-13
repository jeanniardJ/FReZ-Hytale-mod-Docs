# Système d'Interface Utilisateur (UI)

Hytale utilise un système d'UI déclaratif puissant qui sépare la mise en page (fichiers `.ui`) de la logique (code Java).

## Architecture Client-Serveur

1.  **Fichiers `.ui` (Client) :** Les fichiers de layout (`.ui`) sont téléchargés sur le client lorsqu'un joueur se connecte. Le serveur ne peut pas créer ces fichiers dynamiquement ; ils doivent exister à l'avance.
2.  **`InteractiveCustomUIPage` (Serveur) :** Une classe Java côté serveur qui contrôle l'interface.
    *   La méthode `build()` est appelée pour construire l'interface, charger le layout, définir des valeurs initiales et lier des événements.
    *   La méthode `handleDataEvent()` est appelée pour réagir aux interactions de l'utilisateur (clics de bouton, etc.).
3.  **Communication :**
    *   Le **serveur** envoie des **commandes** au client pour manipuler l'interface (par exemple, changer un texte, afficher un panneau).
    *   Le **client** renvoie des **événements** au serveur lorsque l'utilisateur interagit avec l'interface.

**Important :** Toute erreur de syntaxe dans un fichier `.ui` provoquera très probablement la déconnexion du joueur lorsqu'il essaiera d'ouvrir l'interface.

## Structure de Projet Recommandée

Pour une bonne organisation, les fichiers UI doivent être placés dans une structure de dossiers spécifique dans `src/main/resources`.

```
src/main/resources/
└── Common/
    └── UI/
        └── Custom/
            └── VotreNomDePlugin/
                └── VotrePage.ui
```

Le code Java pour charger cette page serait alors :
```java
// Le chemin est relatif au dossier Common/UI/Custom/
cmd.append("VotreNomDePlugin/VotrePage.ui"); 
```

## Configuration du `manifest.json`

Pour que le serveur reconnaisse et serve les fichiers `.ui` au client, votre `manifest.json` doit inclure :
```json
{
  "IncludesAssetPack": true
}
```
