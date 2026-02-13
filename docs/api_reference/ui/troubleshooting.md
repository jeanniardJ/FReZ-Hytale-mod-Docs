# Dépannage de l'Interface Utilisateur (UI)

Guide de débogage pour les erreurs d'UI Hytale (Croix Rouge, plantages du parser, etc.)

## 1. Le Plantage "Unknown Node Type" (Type de Nœud Inconnu)

**Log d'Erreur :**
`HytaleClient.Interface.UI.Markup.TextParser+TextParserException: Failed to parse file ... – Unknown node type: Image`

**Pourquoi ça arrive :**
Le parser du client Hytale dans certaines versions ne supporte pas la balise `<Image>` comme élément autonome, ou ne la supporte que dans des conteneurs parents spécifiques.

**La Correction :**
Standardisez votre UI pour utiliser des **`Group` avec des `Background`** au lieu de nœuds `Image`. C'est fonctionnellement identique mais sécuritaire pour le parser.

*   ❌ **À éviter :** `Image { TexturePath: "..."; }`
*   ✅ **À utiliser :** `Group { Background: ( TexturePath: "..." ); }`

## 2. La Boucle de Plantage "Failed to Apply CustomUI"

**Symptômes :**

*   Le jeu saccade toutes les X secondes.
*   Le client finit par se déconnecter avec "Failed to load CustomUI documents".
*   La console est inondée d'erreurs de paquets.

**Pourquoi ça arrive :**
Votre code Java renvoie le fichier UI entier (`builder.append(...)`) à chaque tick de mise à jour (par exemple, dans une tâche planifiée). Recharger le DOM de manière répétée corrompt l'état de l'UI du client.

**La Correction :**
Implémentez le **Pattern de Séparation Chargement-Mise à Jour** :

1.  **Initialisation :** Envoyez la structure une seule fois.
2.  **Mise à jour :** Envoyez uniquement les changements de variables en utilisant `update(false, builder)`.

## 3. La "Croix Rouge" (Échec de Résolution d'Asset)

**Symptômes :**

*   L'UI se charge physiquement mais toutes les textures sont remplacées par de grandes croix rouges.

**Pourquoi ça arrive :**
Le client ne peut pas trouver le fichier au chemin spécifié. C'est généralement un problème de contexte de chemin.

**La Correction :**

1.  **Localité :** Déplacez les assets dans un sous-dossier (par ex., `Assets/`) **directement à l'intérieur** du dossier contenant votre fichier `.ui`.
2.  **Chemins Relatifs :** Référencez-les simplement comme `"Assets/MyTexture.png"`.
3.  **Manifeste :** Assurez-vous que `manifest.json` contient `"IncludesAssetPack": true`.

## 5. "Failed to load CustomUI documents" (Erreur de Parsing)

**Symptômes :**
*   Le client se déconnecte immédiatement à l'ouverture de l'UI.
*   Le log affiche `Crash - Failed to load CustomUI documents`.

**Causes & Corrections :**

1.  **Commentaires (`//`)** : Le parser peut échouer si des commentaires sont présents, surtout sur la même ligne que des propriétés.
    *   **Correction** : Supprimez tous les commentaires des fichiers `.ui`.

2.  **Imports Cassés** : Définir une variable qui pointe vers un fichier manquant (par ex., `$C = "../Common.ui";`) provoque un plantage, même si la variable n'est pas utilisée.
    *   **Correction** : Supprimez ou commentez les imports vers des fichiers manquants.

3.  **Types de Style Manquants** : Définir un style complexe sans son enveloppe de type cause une erreur de parsing.
    *   ❌ **Faux** : `Style: ( Default: (...) );`
    *   ✅ **Correct** : `Style: TextButtonStyle( Default: (...) );`

4.  **Styles en Ligne** : Utiliser des variables (`@Style`) peut parfois être instable.
    *   **Correction** : Définissez les styles directement dans le composant : `Style: LabelStyle(FontSize: 14, ...);`
