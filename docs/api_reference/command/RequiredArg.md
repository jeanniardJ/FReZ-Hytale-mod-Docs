# RequiredArg

`com.hypixel.hytale.server.core.command.system.arguments.system.RequiredArg`

Une classe représentant un argument requis dans la structure d'une commande.

## Définition

```java
public class RequiredArg<DataType> extends Argument<RequiredArg<DataType>, DataType>
```

## Constructeur

```java
public RequiredArg(@Nonnull AbstractCommand commandRegisteredTo, @Nonnull String name, @Nonnull String description, @Nonnull ArgumentType<DataType> argumentType)
```

* **commandRegisteredTo**: La commande à laquelle cet argument appartient.
* **name**: Le nom de l'argument.
* **description**: Une description de l'argument.
* **argumentType**: Le type de l'argument (doit avoir au moins 1 paramètre).

Lève une `IllegalArgumentException` si `argumentType.getNumberOfParameters() < 1`.

## Méthodes

### getUsageMessageWithoutDescription

```java
@Nonnull
public Message getUsageMessageWithoutDescription()
```
Retourne le format d'utilisation comme `<nom:type>`.

### getUsageMessage

```java
@Nonnull
public Message getUsageMessage()
```
Retourne l'utilisation détaillée comme `nom (type) -> "description"`.

### getUsageOneLiner

```java
@Nonnull
public Message getUsageOneLiner()
```
Retourne l'utilisation courte comme `<nom>`.
