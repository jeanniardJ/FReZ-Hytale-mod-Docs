# Système de Permissions

Le système de permissions de Hytale permet de contrôler l'accès aux commandes et aux fonctionnalités.

## Définir les Permissions

Dans vos classes de commande (héritant de `CommandBase` ou `AbstractCommand`), vous pouvez définir les permissions requises dans le constructeur.

```java
public class MyCommand extends CommandBase {
    public MyCommand() {
        super("mycommand", "description");

        // Permission explicite
        requirePermission("hytale.custom.mycommand");

        // Alternativement, utiliser l'aide pour le format standard
        requirePermission(HytalePermissions.fromCommand("mycommand"));
    }
}
```

## Génération de Permissions

Si aucune permission n'est explicitement définie, les permissions sont générées automatiquement :

* **Commandes système** : `hytale.system.command.<nom>`
* **Commandes de plugin** : `<plugin.basepermission>.command.<nom>`
* **Sous-commandes** : `<parent.permission>.<nom>`

## Groupes de Permissions

Vous pouvez assigner des commandes à des groupes de permissions basés sur le mode de jeu.

```java
// Assigner au groupe de permission du mode Aventure
setPermissionGroup(GameMode.Adventure);
setPermissionGroup(GameMode.Creative);

// Assigner à plusieurs groupes
setPermissionGroups("Adventure", "Creative");
```

## Permissions au Niveau des Arguments

Vous pouvez restreindre l'utilisation de certains arguments à des permissions spécifiques.

```java
private final OptionalArg<PlayerRef> playerArg =
    withOptionalArg("player", "desc", ArgTypes.PLAYER_REF)
        .setPermission("mycommand.target.other");
```

## Vérification des Permissions

Vous pouvez vérifier les permissions manuellement dans votre code.

```java
// Pendant l'exécution d'une commande
if (context.sender().hasPermission("some.permission")) {
    // Permission accordée
}

// Méthode utilitaire (lance une NoPermissionException si la permission est refusée)
CommandUtil.requirePermission(context.sender(), "some.permission");
```
