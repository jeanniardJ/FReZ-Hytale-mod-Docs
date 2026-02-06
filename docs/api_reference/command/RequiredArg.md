# RequiredArg

`com.hypixel.hytale.server.core.command.system.arguments.system.RequiredArg`

A class representing a required argument in a command structure.

## Definition

```java
public class RequiredArg<DataType> extends Argument<RequiredArg<DataType>, DataType>
```

## Constructor

```java
public RequiredArg(@Nonnull AbstractCommand commandRegisteredTo, @Nonnull String name, @Nonnull String description, @Nonnull ArgumentType<DataType> argumentType)
```

* **commandRegisteredTo**: The command this argument belongs to.
* **name**: The name of the argument.
* **description**: A description of the argument.
* **argumentType**: The type of the argument (must have at least 1 parameter).

Throws `IllegalArgumentException` if `argumentType.getNumberOfParameters() < 1`.

## Methods

### getUsageMessageWithoutDescription

```java
@Nonnull
public Message getUsageMessageWithoutDescription()
```
Returns usage format like `<name:type>`.

### getUsageMessage

```java
@Nonnull
public Message getUsageMessage()
```
Returns detailed usage like `name (type) -> "description"`.

### getUsageOneLiner

```java
@Nonnull
public Message getUsageOneLiner()
```
Returns short usage like `<name>`.
