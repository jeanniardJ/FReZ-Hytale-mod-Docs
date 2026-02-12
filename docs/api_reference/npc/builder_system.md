# ⚙️ Système de "Builder" de PNJ

Ce document décrit les composants internes et plus techniques du système de "builder" utilisé pour configurer les PNJ.

## `SubTypeTypeAdapterFactory`

C'est une classe utilitaire très technique qui s'intègre avec la librairie **Gson** pour gérer la sérialisation d'objets polymorphiques en JSON.

### Contexte : Le Problème du Polymorphisme en JSON

Imaginons que vous ayez une classe de base `Action` et plusieurs sous-classes comme `ActionSay`, `ActionWalk`, `ActionAttack`. Si vous avez une liste `List<Action>`, comment Gson peut-il savoir comment écrire chaque élément en JSON, et surtout, comment savoir de quel type il s'agissait à la relecture ?

`SubTypeTypeAdapterFactory` résout la partie "écriture" de ce problème.

### Rôle

Cette "factory" crée un `TypeAdapter` pour Gson qui ajoute automatiquement un champ supplémentaire au JSON de sortie pour identifier le type de la sous-classe.

### Fonctionnement

1.  **Création** : On l'initialise pour une classe de base et avec le nom du champ qui identifiera le type.
    ```java
    // Pour la classe de base 'Action', le champ de type sera 'action_type'
    SubTypeTypeAdapterFactory factory = SubTypeTypeAdapterFactory.of(Action.class, "action_type");
    ```

2.  **Enregistrement des sous-types** : On doit ensuite déclarer chaque sous-classe et le nom qui lui sera associé dans le JSON.
    ```java
    factory.registerSubType(ActionSay.class, "say");
    factory.registerSubType(ActionWalk.class, "walk");
    ```

3.  **Utilisation avec Gson** : On ajoute cette factory à notre instance de Gson.
    ```java
    Gson gson = new GsonBuilder()
        .registerTypeAdapterFactory(factory)
        .create();
    ```

4.  **Résultat à la sérialisation** : Quand on sérialise un objet `ActionSay`, le JSON ressemblera à ceci :
    ```json
    {
      "action_type": "say",
      // ... autres propriétés de ActionSay
    }
    ```

**Important** : Cette implémentation ne gère **que l'écriture** (sérialisation). La méthode `read()` lance une exception. Un autre mécanisme est nécessaire pour lire ce JSON polymorphique.

## `NPCRoleValidator`

Ce validateur est utilisé pour s'assurer que les références de "rôle" de PNJ spécifiées dans les fichiers de configuration sont valides.

*   **Objectif** : Empêcher les erreurs de configuration en vérifiant que le `Role` (rôle) qu'un PNJ essaie d'utiliser existe bien et est valide.
*   **Fonctionnement** : Lorsqu'une chaîne de caractères (qui est censée être le nom d'un rôle de PNJ) est validée, ce validateur appelle `NPCPlugin.get().validateSpawnableRole(roleName)`. Si cette méthode lève une exception (par exemple, si le rôle n'existe pas ou n'est pas correctement défini), la validation échoue.

Ce type de validateur est crucial pour un système robuste, car il permet de détecter les erreurs de configuration *avant* que le jeu ne tente de les utiliser, évitant ainsi des crashs ou des comportements inattendus.
