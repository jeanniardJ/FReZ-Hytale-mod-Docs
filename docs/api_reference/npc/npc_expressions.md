# 📈 Langage d'Expression pour PNJ

Le système de PNJ inclut un langage d'expression simple qui permet d'écrire des logiques plus dynamiques, probablement pour des conditions ou des calculs complexes dans les fichiers de configuration.

Ce document décrit les composants de ce langage.

## `ValueType` (Types de Données)

Le langage d'expression est typé. L'`enum` `ValueType` définit tous les types de données qui peuvent être manipulés.

*   `VOID`: Représente l'absence de valeur (similaire à `void` en Java).
*   `NUMBER`: Un nombre (probablement un `double` ou `float` en interne).
*   `STRING`: Une chaîne de caractères.
*   `BOOLEAN`: Une valeur booléenne (`true` ou `false`).

### Types Tableaux

Le langage supporte également les tableaux :

*   `NUMBER_ARRAY`: Un tableau de nombres.
*   `STRING_ARRAY`: Un tableau de chaînes de caractères.
*   `BOOLEAN_ARRAY`: Un tableau de booléens.
*   `EMPTY_ARRAY`: Un type spécial pour un tableau vide, qui peut être assigné à n'importe quel type de tableau.

### Assignation de Type

La méthode `isAssignableType(from, to)` définit les règles de compatibilité. Par exemple, on peut assigner un `NUMBER` à une variable de type `NUMBER`, mais pas un `STRING`. La seule exception est qu'un `EMPTY_ARRAY` peut être assigné à n'importe quel `..._ARRAY`.

## Compilation : `Token` et `TokenFlags`

Pour que le moteur du jeu puisse comprendre une expression comme `"Hello" + " " + world.name`, il doit d'abord la décomposer en "jetons" (tokens). C'est la première étape de la compilation, appelée l'analyse lexicale (ou "lexing").

### `Token`

L'`enum` `Token` représente toutes les briques de base du langage.

*   **Opérandes** : Les valeurs sur lesquelles on opère.
    *   `NUMBER`: `123`, `45.6`
    *   `STRING`: `"Hello"`
    *   `IDENTIFIER`: `world.name`, `player.health`

*   **Opérateurs** : Les symboles qui effectuent des opérations.
    *   Arithmétiques : `+`, `-`, `*`, `/`, `%` (reste), `**` (puissance).
    *   Logiques : `&&` (et), `||` (ou), `!` (non).
    *   Comparaison : `==`, `!=`, `>`, `<`, `>=`, `<=`.

*   **Parenthèses et Crochets** : `(`, `)`, `[`, `]`.

*   **Autres** : `COMMA` (virgule, pour les listes ou les arguments de fonction), `FUNCTION_CALL`.

Chaque `Token` possède des métadonnées importantes pour la suite de la compilation (l'analyse syntaxique ou "parsing") :

*   **`precedence`** (Priorité) : Définit l'ordre des opérations. Par exemple, `*` a une priorité plus élevée que `+`, donc `2 + 3 * 4` est évalué comme `2 + 12`.
*   **`flags`** : Un ensemble de `TokenFlags` qui décrivent la nature du jeton (est-ce un opérateur ? un opérande ? un opérateur unaire comme le `-` dans `-5` ?).

### `TokenFlags`

Cette `enum` définit les différentes propriétés (les "drapeaux") qu'un `Token` peut avoir. Ces drapeaux sont utilisés par l'analyseur syntaxique pour déterminer comment interpréter chaque jeton.

*   `OPERAND`: Indique que le jeton est une valeur sur laquelle on peut opérer (par exemple, un nombre, une chaîne de caractères, une variable).
*   `LITERAL`: Indique que l'opérande est une valeur constante directement écrite dans le code (ex: `10`, `"Bonjour"`).
*   `OPERATOR`: Indique que le jeton est un opérateur (ex: `+`, `&&`).
*   `RIGHT_TO_LEFT`: Indique que l'opérateur a une associativité de droite à gauche. (ex: `a ** b ** c` est `a ** (b ** c)`).
*   `UNARY`: Indique que l'opérateur est unaire (il s'applique à un seul opérande, ex: le `-` dans `-5`).
*   `OPENING_BRACKET`, `CLOSING_BRACKET`: Indique une parenthèse ou un crochet ouvrant/fermant.
*   `LIST`: Indique un séparateur de liste (comme la virgule `,`).
*   `OPENING_TUPLE`: Indique le début d'un tuple (comme le `[` pour un tableau).

