# Référence Complète de l'UI

Le guide ultime et complet du système d'UI de Hytale - syntaxe, composants, propriétés, styles, mises en page et API Java.

## Table des Matières

1. [Aperçu de la Syntaxe DSL](#1-aperçu-de-la-syntaxe-dsl)
2. [Imports et Variables](#2-imports-et-variables)
3. [Types de Composants](#3-types-de-composants)
4. [Référence des Propriétés](#4-référence-des-propriétés)
5. [Système d'Ancrage (Positionnement)](#5-système-dancrage-positionnement)
6. [Modes de Mise en Page](#6-modes-de-mise-en-page)
7. [Système de Style](#7-système-de-style)
8. [Formats de Couleur](#8-formats-de-couleur)
9. [Types de Valeurs](#9-types-de-valeurs)
10. [Templates et Héritage](#10-templates-et-héritage)
11. [Liaisons d'Événements](#11-liaisons-dévénements)
12. [API Java](#12-api-java)
13. [Bibliothèque Common.ui](#13-bibliothèque-commonui)
14. [Exemples Complets](#14-exemples-complets)

---

## 1. Aperçu de la Syntaxe DSL

Hytale utilise un langage dédié (DSL) pour les définitions d'UI - ce n'est **ni du XML, ni du JSON, ni du XAML**.

### Structure de Base

```
// Imports
$C = "../Common.ui";

// Variables
@MyColor = #3a7bd5;

// Styles
@MyStyle = LabelStyle(FontSize: 14, TextColor: #ffffff);

// Composants
Group #RootContainer {
  Anchor: (Width: 400, Height: 300);
  Background: #1a1a2e(0.95);

  Label #Title {
    Text: "Hello World";
  }
}
```

### Règles de Syntaxe

| Élément | Préfixe | Exemple |
|---|---|---|
| Alias d'import | `$` | `$C = "../Common.ui";` |
| Variable/Style | `@` | `@MyColor = #ff0000;` |
| ID d'élément | `#` | `Label #Title { }` |
| Couleur | `#` | `#3a7bd5` ou `#3a7bd5(0.95)` |
| Réf. de template | `$Alias.@Name` | `$C.@TextButton` |
| Référence | `@Name` | `Style: @MyStyle;` |
| Spread (Étalement) | `...` | `...@BaseStyle` |
| Localisé | `%` | `%server.customUI.key.path` |

---

## 2. Imports et Variables

### Imports de Fichiers

```
$C = "../Common.ui";
$Sounds = "Sounds.ui";
$Styles = "../styles/Theme.ui";
```

Accéder au contenu importé : `$C.@TextButton`, `$Sounds.@ButtonSounds`

### Définitions de Variables

```
// Couleurs
@PrimaryColor = #3a7bd5;
@TextColor = #ffffff;
@DisabledColor = #6b7280(0.5);

// Nombres
@ButtonHeight = 44;
@DefaultPadding = 16;

// Booléens
@ShowHeader = true;

// Chaînes de caractères
@DefaultText = "Click here";

// Objets
@DefaultAnchor = (Width: 100, Height: 40);

// Références à d'autres variables
@SecondaryColor = @PrimaryColor;
```

---

## 3. Types de Composants

### Composants de Mise en Page

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Group` | Conteneur principal pour organiser les éléments | `LayoutMode`, `Padding`, `Background` |
| `Container` | Conteneur générique | `LayoutMode`, `Padding` |
| `Panel` | Panneau conteneur | `Background`, `BorderRadius` |
| `Box` | Boîte flexible | `FlexWeight` |
| `HorizontalGroup` | Groupe à mise en page horizontale | `Gap`, `Alignment` |
| `VerticalGroup` | Groupe à mise en page verticale | `Gap`, `Alignment` |
| `ScrollView` | Conteneur à défilement | `ScrollbarStyle` |
| `ScrollPanel` | Panneau à défilement | `ScrollbarStyle` |
| `Grid` | Mise en page en grille | `Columns`, `Gap` |
| `Stack` | Empilement sur l'axe Z | - |
| `Frame` | Conteneur de cadre | `Background` |

### Composants de Texte

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Label` | Affichage de texte | `Text`, `Style` (LabelStyle) |
| `Text` | Élément de texte | `Text`, `Style` |
| `RichText` | Texte formaté riche | `Text`, `Wrap` |

### Composants de Saisie

| Composant | Description | Propriétés Clés |
|---|---|---|
| `TextField` | Saisie de texte sur une seule ligne | `PlaceholderText`, `Value`, `Style` |
| `TextInput` | Élément de saisie de texte | `PlaceholderText`, `MaxLength` |
| `TextArea` | Saisie de texte sur plusieurs lignes | `PlaceholderText`, `Wrap` |
| `NumberField` | Saisie numérique | `Min`, `Max`, `Step`, `Value`, `Format` |
| `CompactTextField` | Saisie de texte extensible | `CollapsedWidth`, `ExpandedWidth` |

### Composants de Bouton

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Button` | Bouton de base | `Style` (ButtonStyle) |
| `TextButton` | Bouton avec texte | `Text`, `Style` (TextButtonStyle) |
| `ImageButton` | Bouton avec image | `Texture`, `Style` |
| `IconButton` | Bouton avec icône uniquement | `Icon`, `Style` |
| `ToggleButton` | Bouton à bascule | `Checked`, `Style` |
| `CheckBox` | Case à cocher | `Checked`, `Style` (CheckBoxStyle) |
| `CheckBoxWithLabel` | Case à cocher avec libellé | `Text`, `Checked` |
| `RadioButton` | Bouton radio | `Group`, `Checked` |
| `BackButton` | Bouton de retour de navigation | `Style` |

### Composants de Sélection

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Dropdown` | Menu déroulant | `Style` (DropdownStyle) |
| `DropdownBox` | Sélection déroulante | `Entries`, `Style` (DropdownBoxStyle) |
| `ComboBox` | Menu déroulant éditable | `Style` |
| `Select` | Menu déroulant de sélection | `Options` |
| `FileDropdownBox` | Menu déroulant de sélection de fichier | `Filter` |

### Composants de Curseur (Slider)

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Slider` | Curseur de valeur | `Min`, `Max`, `Value`, `Step`, `Style` |
| `FloatSlider` | Curseur de valeur flottante | `Min`, `Max`, `Value`, `Step` |
| `Scrollbar` | Élément de barre de défilement | `Style` (ScrollbarStyle) |
| `ProgressBar` | Indicateur de progression | `Value`, `Max`, `Style` |

### Composants d'Image

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Image` | Affichage d'image (**déprécié**) | Utilisez `Group` avec `Background` à la place |
| `Icon` | Affichage d'icône | `TexturePath`, `Tint` |
| `Sprite` | Sprite animé | `TexturePath`, `Frame`, `FramesPerSecond` |
| `NineSlice` | Image redimensionnable (9-patch) | `TexturePath`, `Border` |

### Composants Spécifiques au Jeu

| Composant | Description | Propriétés Clés |
|---|---|---|
| `ItemSlot` | Emplacement d'inventaire | `Background`, `Overlay`, `Icon` |
| `ItemGrid` | Grille d'emplacements d'objets | `SlotsPerRow`, `Slots` |
| `ItemIcon` | Affichage d'icône d'objet | `Item` |
| `Inventory` | Conteneur d'inventaire | `Slots` |
| `InventorySlot` | Emplacement d'inventaire unique | `Index` |
| `Hotbar` | Conteneur de barre de raccourcis | `Slots` |
| `HotbarSlot` | Emplacement de barre de raccourcis unique | `Index` |
| `Tooltip` | Info-bulle contextuelle | `Style` (TextTooltipStyle) |
| `TooltipPanel` | Panneau d'info-bulle | `Style` |

### Composants Utilitaires

| Composant | Description | Propriétés Clés |
|---|---|---|
| `Divider` | Séparateur horizontal | `Color`, `Thickness` |
| `Separator` | Séparateur visuel | `Orientation` |
| `Spacer` | Espace vide | `Width`, `Height` |
| `ColorPicker` | Sélecteur de couleur | `Value`, `Style` (ColorPickerStyle) |

---

## 4. Référence des Propriétés

### Propriétés d'Ancrage

| Propriété | Type | Description | Exemple |
|---|---|---|---|
| `Anchor` | Objet | Conteneur pour le positionnement | `Anchor: (Width: 100, Height: 50)` |
| `Left` | Nombre | Distance du bord gauche | `Left: 10` |
| `Right` | Nombre | Distance du bord droit | `Right: 10` |
| `Top` | Nombre | Distance du bord supérieur | `Top: 20` |
| `Bottom` | Nombre | Distance du bord inférieur | `Bottom: 20` |
| `Width` | Nombre | Largeur explicite | `Width: 100` |
| `Height` | Nombre | Hauteur explicite | `Height: 50` |
| `MinWidth` | Nombre | Largeur minimale | `MinWidth: 50` |
| `MaxWidth` | Nombre | Largeur maximale | `MaxWidth: 500` |
| `MinHeight` | Nombre | Hauteur minimale | `MinHeight: 30` |
| `MaxHeight` | Nombre | Hauteur maximale | `MaxHeight: 100` |
| `Horizontal` | Nombre | Étirement horizontal | `Horizontal: 0` |
| `Vertical` | Nombre | Étirement vertical | `Vertical: 0` |
| `Full` | Nombre | Remplir le parent | `Full: 0` |

### Propriétés de Mise en Page

| Propriété | Type | Options | Description |
|---|---|---|---|
| `LayoutMode` | Enum | `Top`, `TopScrolling`, `Left`, `Right`, `Center`, `Bottom`, `Overlay`, `Absolute` | Agencement des enfants |
| `HorizontalAlignment` | Enum | `Left`, `Center`, `Right`, `Stretch` | Alignement horizontal |
| `VerticalAlignment` | Enum | `Top`, `Center`, `Bottom`, `Stretch` | Alignement vertical |
| `FlexWeight` | Nombre | Poids de dimensionnement flexible | |
| `Gap` | Nombre | Espace entre les enfants | |
| `Spacing` | Nombre | Espacement des éléments | |

### Propriétés de Marge Intérieure (Padding)

| Propriété | Type | Description |
|---|---|---|
| `Padding` | Objet/Nombre | Marge intérieure |
| `Padding.Full` | Nombre | Tous les côtés |
| `Padding.Horizontal` | Nombre | Gauche + Droite |
| `Padding.Vertical` | Nombre | Haut + Bas |
| `Padding.Top` | Nombre | Haut uniquement |
| `Padding.Bottom` | Nombre | Bas uniquement |
| `Padding.Left` | Nombre | Gauche uniquement |
| `Padding.Right` | Nombre | Droite uniquement |

### Propriétés Visuelles

| Propriété | Type | Description | Exemple |
|---|---|---|---|
| `Background` | Couleur/PatchStyle | Arrière-plan | `#1a1a2e` ou `(TexturePath: "...", Border: 16)` |
| `Foreground` | Couleur | Couleur de premier plan | `#ffffff` |
| `Color` | Couleur | Couleur de l'élément | `#3a7bd5` |
| `Opacity` | Nombre | Transparence (0-1) | `0.95` |
| `Alpha` | Nombre | Valeur alpha (0-1) | `0.8` |
| `Visible` | Booléen | Visibilité | `true` |
| `Enabled` | Booléen | État activé | `true` |
| `BorderColor` | Couleur | Couleur de la bordure | `#96a9be` |
| `BorderWidth` | Nombre | Épaisseur de la bordure | `2` |
| `CornerRadius` | Nombre | Arrondi des coins | `6` |
| `HitTestVisible` | Booléen | Cliquable | `true` |

### Propriétés de Texte

| Propriété | Type | Description | Exemple |
|---|---|---|---|
| `Text` | Chaîne | Contenu textuel | `"Hello"` |
| `TextColor` | Couleur | Couleur du texte | `#ffffff` |
| `FontSize` | Nombre | Taille de la police (px) | `14` |
| `FontName` | Chaîne | Nom de la police | `"Default"`, `"Secondary"` |
| `RenderBold` | Booléen | Texte en gras | `true` |
| `RenderItalic` | Booléen | Texte en italique | `false` |
| `RenderUppercase` | Booléen | En majuscules | `true` |
| `Wrap` | Booléen | Retour à la ligne | `true` |
| `LineHeight` | Nombre | Hauteur de ligne | `1.5` |
| `LetterSpacing` | Nombre | Espacement des lettres | `0.5` |
| `OutlineColor` | Couleur | Contour du texte | `#000000(0.5)` |

### Propriétés d'Image

| Propriété | Type | Description | Exemple |
|---|---|---|---|
| `TexturePath` | Chaîne | Chemin de la texture | `"Common/Button.png"` |
| `Texture` | Chaîne | Référence de texture | `"@ButtonTexture"` |
| `Border` | Nombre | Bordure 9-patch | `16` |
| `HorizontalBorder` | Nombre | Bordure horizontale | `80` |
| `VerticalBorder` | Nombre | Bordure verticale | `20` |
| `Tint` | Couleur | Teinte de couleur | `#ff0000` |
| `Frame` | Objet | Trame d'animation | `(Width: 32, Height: 32, PerRow: 8, Count: 72)` |
| `FramesPerSecond` | Nombre | FPS de l'animation | `30` |
| `Area` | Objet | Région de la texture | `(X: 0, Y: 0, Width: 100, Height: 100)` |

### Propriétés de Saisie

| Propriété | Type | Description | Exemple |
|---|---|---|---|
| `PlaceholderText` | Chaîne | Texte indicatif | `"Enter text..."` |
| `PlaceholderStyle` | LabelStyle | Style du texte indicatif | `@PlaceholderStyle` |
| `Value` | Nombre/Chaîne | Valeur actuelle | `50` |
| `Min` | Nombre | Valeur minimale | `0` |
| `Max` | Nombre | Valeur maximale | `100` |
| `Step` | Nombre | Pas de la valeur | `5` |
| `Format` | Objet | Format du nombre | `(MaxDecimalPlaces: 0)` |
| `MaxLength` | Nombre | Longueur max du texte | `100` |

---

## 5. Système d'Ancrage (Positionnement)

Hytale utilise un **système de positionnement basé sur les bords**, similaire au positionnement absolu en CSS.

### Positionnement de Base

```
// Taille fixe à une position donnée
Anchor: (Left: 10, Top: 20, Width: 100, Height: 50);

// Taille uniquement (mise en page fluide)
Anchor: (Width: 400, Height: 300);

// Position par rapport aux bords
Anchor: (Left: 50, Top: 170);
```

### Modes d'Étirement

```
// Pleine largeur, hauteur fixe
Anchor: (Left: 0, Right: 0, Height: 50);

// Pleine hauteur, largeur fixe
Anchor: (Top: 0, Bottom: 0, Width: 200);

// Remplir tout le parent
Anchor: (Full: 0);

// Étirement horizontal
Anchor: (Horizontal: 0, Height: 50);

// Étirement vertical
Anchor: (Vertical: 0, Width: 100);
```

### Décalages Négatifs

```
// Positionner en dehors des limites du parent
Anchor: (Width: 32, Height: 32, Top: -16, Right: -16);
```

### Contraintes de Taille

```
Anchor: (
  Width: 200,
  Height: 100,
  MinWidth: 100,
  MaxWidth: 400,
  MinHeight: 50,
  MaxHeight: 200
);
```

---

## 6. Modes de Mise en Page

| Mode | Comportement | Cas d'Usage |
|---|---|---|
| `Top` | Enfants empilés verticalement depuis le haut | Listes verticales, formulaires |
| `TopScrolling` | Vertical avec défilement | Listes à défilement |
| `Bottom` | Enfants empilés depuis le bas | Contenu aligné en bas |
| `Left` | Enfants arrangés horizontalement depuis la gauche | Barres d'outils, rangées de boutons |
| `Right` | Enfants arrangés depuis la droite | Mises en page RTL |
| `Center` | Enfants centrés horizontalement | Boutons centrés |
| `Overlay` | Enfants superposés (empilement Z) | Pop-ups, modales |
| `Absolute` | Enfants utilisant le positionnement `Anchor` | Mises en page libres |

```
Group {
  LayoutMode: Top;
  // Les enfants s'empilent verticalement
}
```

---

## 7. Système de Style

### Types de Style

| Type de Style | Utilisé Par | États |
|---|---|---|
| `LabelStyle` | Label, Text | Default, Disabled |
| `TextButtonStyle` | TextButton | Default, Hovered, Pressed, Disabled |
| `ButtonStyle` | Button, ImageButton | Default, Hovered, Pressed, Disabled |
| `CheckBoxStyle` | CheckBox | Unchecked, Checked (avec sous-états) |
| `DropdownBoxStyle` | DropdownBox | Multiples sous-styles |
| `SliderStyle` | Slider | Default, Hovered, Pressed |
| `ScrollbarStyle` | Scrollbar, ScrollView | Default, Hovered, Dragged |
| `InputFieldStyle` | TextField | Default, Focused, Disabled |
| `PatchStyle` | Arrière-plans 9-Slice | - |
| `TextTooltipStyle` | Tooltip | Default |
| `ColorPickerStyle` | ColorPicker | - |
| `PopupMenuLayerStyle` | Menus | Multiples sous-styles |
| `TabNavigationStyle` | Onglets | États des onglets, états sélectionnés |

### LabelStyle

```
@MyLabelStyle = LabelStyle(
  FontSize: 14,
  FontName: "Default",
  TextColor: #ffffff,
  OutlineColor: #000000(0.3),
  HorizontalAlignment: Center,
  VerticalAlignment: Center,
  RenderBold: true,
  RenderUppercase: false,
  LetterSpacing: 0,
  LineSpacing: 1.0,
  Wrap: true
);
```

### TextButtonStyle

```
@MyButtonStyle = TextButtonStyle(
  Default: (
    Background: #3a7bd5,
    LabelStyle: (FontSize: 14, TextColor: #ffffff, RenderBold: true)
  ),
  Hovered: (
    Background: #2563eb,
    LabelStyle: (TextColor: #ffffff)
  ),
  Pressed: (
    Background: #1e40af,
    LabelStyle: (TextColor: #e0e7ff)
  ),
  Disabled: (
    Background: #6b7280,
    LabelStyle: (TextColor: #9ca3af)
  ),
  Sounds: @ButtonSounds
);
```

### PatchStyle (9-Slice)

```
@MyBackground = PatchStyle(
  TexturePath: "Common/Panel.png",
  Border: 16
);

// Avec des bordures horizontales/verticales différentes
@MyButton = PatchStyle(
  TexturePath: "Common/Button.png",
  HorizontalBorder: 80,
  VerticalBorder: 12,
  Color: #ffffff(0.95)
);

// Avec une région de texture
@MyPatch = PatchStyle(
  TexturePath: "Common/Atlas.png",
  Border: 8,
  Area: (X: 0, Y: 0, Width: 64, Height: 64)
);
```

### CheckBoxStyle

```
@MyCheckBoxStyle = CheckBoxStyle(
  Unchecked: (
    DefaultBackground: (Color: #2b3542),
    HoveredBackground: (Color: #3a4556),
    PressedBackground: (Color: #1e2633),
    DisabledBackground: (Color: #1a1a1a),
    ChangedSound: (SoundPath: "Sounds/Click.ogg", Volume: 6)
  ),
  Checked: (
    DefaultBackground: (TexturePath: "Common/CheckMark.png"),
    HoveredBackground: (TexturePath: "Common/CheckMarkHover.png"),
    PressedBackground: (TexturePath: "Common/CheckMarkPressed.png"),
    ChangedSound: (SoundPath: "Sounds/Click.ogg", Volume: 6)
  )
);
```

### ScrollbarStyle

```
@MyScrollbarStyle = ScrollbarStyle(
  Spacing: 6,
  Size: 6,
  OnlyVisibleWhenHovered: true,
  Background: (TexturePath: "Common/ScrollbarBg.png", Border: 3),
  Handle: (TexturePath: "Common/ScrollbarHandle.png", Border: 3),
  HoveredHandle: (TexturePath: "Common/ScrollbarHandleHover.png", Border: 3),
  DraggedHandle: (TexturePath: "Common/ScrollbarHandleDrag.png", Border: 3)
);
```

### Héritage de Style (Spread)

```
// Style de base
@BaseButtonStyle = TextButtonStyle(
  Default: (Background: #3a7bd5, LabelStyle: @DefaultLabel),
  Hovered: (Background: #2563eb),
  Pressed: (Background: #1e40af),
  Disabled: (Background: #6b7280)
);

// Style étendu avec surcharges
@SecondaryButtonStyle = TextButtonStyle(
  ...@BaseButtonStyle,
  Default: (
    ...@BaseButtonStyle.Default,
    Background: #e94560
  )
);
```

---

## 8. Formats de Couleur

### Couleur Hex de Base

```
Background: #3a7bd5;
TextColor: #ffffff;
BorderColor: #000000;
```

### Hex avec Alpha

```
Background: #1a1a2e(0.95);    // 95% opacité
TextColor: #ffffff(0.6);       // 60% opacité
Overlay: #000000(0.5);         // 50% opacité (noir semi-transparent)
OutlineColor: #000000(0.3);    // 30% opacité
```

### Valeurs Alpha

* `0.0` = Complètement transparent
* `0.5` = 50% opacité
* `1.0` = Complètement opaque (défaut)

---

## 9. Types de Valeurs

| Type | Syntaxe | Exemple |
|---|---|---|
| Chaîne | `"texte"` | `Text: "Hello"` |
| Nombre | `123` ou `3.14` | `Width: 100`, `Opacity: 0.95` |
| Pourcentage | `50%` | `Width: 50%` |
| Booléen | `true`/`false` | `Visible: true` |
| Couleur | `#RRGGBB` ou `#RRGGBB(alpha)` | `#3a7bd5(0.95)` |
| Enum | `EnumValue` | `LayoutMode: Top` |
| Objet | `(clé: valeur, ...)` | `Anchor: (Width: 100)` |
| Référence | `@Name` | `Style: @MyStyle` |
| Template | `$Alias.@Name` | `$C.@TextButton` |
| Spread | `...@Name` | `...@BaseStyle` |
| Localisé | `%key.path` | `%server.customUI.title` |

---

## 10. Templates et Héritage

### Utilisation des Templates

```
// Importer le fichier
$C = "../Common.ui";

// Utiliser le template avec des surcharges de paramètres
$C.@TextButton #MyButton {
  @Text = "Click Me";           // Paramètre de template
  Anchor: (Width: 120);         // Propriété du composant
}

$C.@Container #MyContainer {
  @ContentPadding = (Full: 20);
  @CloseButton = true;

  Label { Text: "Content"; }
}
```

### Paramètre de Template vs Propriété

```
$C.@TextButton {
  @Text = "Label";         // @ = Paramètre de template (défini dans le template)
  Anchor: (Width: 100);    // Pas de @ = Propriété du composant
}
```

### Opérateur Spread

```
// Fusionner et surcharger les styles
@ExtendedStyle = LabelStyle(
  ...@BaseStyle,          // Inclure toutes les propriétés de BaseStyle
  FontSize: 18,           // Surcharger FontSize
  TextColor: #ff0000      // Surcharger TextColor
);

// "Spreading" imbriqué
Style: (
  ...@DefaultStyle,
  Sounds: (
    ...$Sounds.@ButtonSounds,
    ...@CustomSounds
  )
);
```

---

## 11. Liaisons d'Événements

### Types d'Événements

| Événement | Description | Cas d'Usage |
|---|---|---|
| `Activating` | Composant activé (clic) | Clics de bouton |
| `RightClicking` | Clic droit de la souris | Menus contextuels |
| `DoubleClicking` | Double clic | Actions rapides |
| `MouseEntered` | Début du survol de la souris | Effets de survol |
| `MouseExited` | Fin du survol de la souris | Nettoyage du survol |
| `ValueChanged` | Valeur d'entrée changée | Saisies de formulaire |
| `ElementReordered` | Élément réordonné | Listes par glisser-déposer |
| `Validating` | Événement de validation | Validation de formulaire |
| `Dismissing` | Fermeture du composant | Fermer les boîtes de dialogue |
| `FocusGained` | L'entrée a le focus | Style de focus |
| `FocusLost` | L'entrée a perdu le focus | Gestion du flou |
| `KeyDown` | Touche pressée | Raccourcis clavier |
| `MouseButtonReleased` | Bouton de souris relâché | Fin du glissement |
| `SlotClicking` | Clic sur un emplacement d'inventaire | Gestion de l'inventaire |
| `SlotDoubleClicking` | Double clic sur un emplacement | Transfert rapide |
| `SlotMouseEntered` | Début du survol d'un emplacement | Info-bulles d'emplacement |
| `SlotMouseExited` | Fin du survol d'un emplacement | Cacher les info-bulles |
| `DragCancelled` | Glissement annulé | Nettoyage du glissement |
| `Dropped` | Objet déposé | Gestion du dépôt |
| `SlotMouseDragCompleted` | Glissement d'emplacement terminé | Transferts d'emplacement |
| `SelectedTabChanged` | Onglet changé | Navigation par onglets |

### Liaison d'Événement en Java

```java
evt.addEventBinding(
    CustomUIEventBindingType.Activating,
    "#MyButton",
    new EventData().append("Action", "buttonClicked")
);

evt.addEventBinding(
    CustomUIEventBindingType.ValueChanged,
    "#MySlider",
    new EventData()
        .append("Action", "sliderChanged")
        .append("Value", "@Value")  // Capturer la valeur actuelle
);
```

---

## 12. API Java

### Classe de Base du Contrôleur de Page

```java
public class MyPageController extends InteractiveCustomUIPage<MyPageController.UIEventData> {

    public static final String LAYOUT = "MyPage.ui";

    public MyPageController(PlayerRef playerRef) {
        super(playerRef, CustomPageLifetime.CanDismiss, UIEventData.CODEC);
    }

    @Override
    public void build(
            Ref<EntityStore> ref,
            UICommandBuilder cmd,
            UIEventBuilder evt,
            Store<EntityStore> store
    ) {
        cmd.append(LAYOUT);
        // Configurer l'UI
    }

    @Override
    public void handleDataEvent(
            Ref<EntityStore> ref,
            Store<EntityStore> store,
            UIEventData data
    ) {
        // Gérer les événements
    }

    public static class UIEventData {
        public static final BuilderCodec<UIEventData> CODEC = /* ... */;
        private String action;
    }
}
```

### Méthodes de `UICommandBuilder`

| Méthode | Description | Exemple |
|---|---|---|
| `append(path)` | Charger l'UI depuis un fichier | `cmd.append("MyPage.ui")` |
| `append(selector, path)` | Ajouter à un élément | `cmd.append("#List", "Item.ui")` |
| `appendInline(selector, doc)` | Ajouter de l'UI en ligne | `cmd.appendInline("#List", "Label { Text: \"...\"; }")` |
| `insertBefore(selector, path)` | Insérer avant l'élément | `cmd.insertBefore("#Item2", "Item.ui")` |
| `remove(selector)` | Supprimer l'élément | `cmd.remove("#OldItem")` |
| `clear(selector)` | Vider les enfants | `cmd.clear("#List")` |
| `set(selector, value)` | Définir une propriété | `cmd.set("#Title.Text", "Hello")` |
| `setNull(selector)` | Définir à `null` | `cmd.setNull("#Value")` |

### Méthodes de `UIEventBuilder`

| Méthode | Description |
|---|---|
| `addEventBinding(type, selector)` | Lier un événement à un élément |
| `addEventBinding(type, selector, data)` | Lier avec capture de données |
| `addEventBinding(type, selector, data, locksInterface)` | Lier avec verrouillage de l'interface |

### Sélecteurs

```java
// Élément par ID
cmd.set("#Title.Text", "Hello");

// Élément indexé
cmd.set("#List[0].Text", "First");
cmd.set("#List[1].Text", "Second");

// Propriété imbriquée
cmd.set("#Button.Style.Background", "#ff0000");
```

### Durées de Vie de Page

| Durée de Vie | Description |
|---|---|
| `CantClose` | Ne peut pas être fermée par l'utilisateur |
| `CanDismiss` | Peut être fermée (Échap) |
| `CanClose` | Peut être fermée (bouton fermer) |
| `CanCloseOrDismiss` | Peut être fermée de l'une ou l'autre manière |

---

## 13. Bibliothèque Common.ui

Le fichier `Common.ui` fournit des templates et des styles pré-construits.

### Templates de Bouton

```
$C.@TextButton #MyButton {
  @Text = "Primary Button";
  Anchor: (Width: 140, Height: 44);
}

$C.@SecondaryTextButton #Secondary {
  @Text = "Secondary";
}

$C.@CancelTextButton #Cancel {
  @Text = "Cancel";
}

$C.@TertiaryTextButton #Tertiary {
  @Text = "Link Style";
}
```

### Templates de Saisie

```
$C.@TextField #NameInput {
  @PlaceholderText = "Enter name...";
  Anchor: (Width: 200, Height: 32);
}

$C.@NumberField #AgeInput {
  @Min = 0;
  @Max = 100;
  @Value = 18;
}

$C.@CheckBoxWithLabel #AgreeCheckbox {
  @Text = "I agree to terms";
  @Checked = false;
}

$C.@DropdownBox #CategorySelect {
  // Les entrées sont définies via Java
}
```

### Templates de Conteneur

```
$C.@Container #Panel {
  @ContentPadding = (Full: 16);
  @CloseButton = true;

  // Contenu ici
}

$C.@DecoratedContainer #FancyPanel {
  @Title = "Panel Title";
  // Contenu ici
}
```

### Templates Utilitaires

```
// Séparateurs
$C.@ContentSeparator {}
$C.@VerticalSeparator {}

// Spinner/Chargement
$C.@DefaultSpinner {}

// Bouton retour
$C.@BackButton #BackBtn {}
```

---

## 14. Exemples Complets

### Boîte de Dialogue Simple

```
$C = "../Common.ui";

@TitleStyle = LabelStyle(
  FontSize: 20,
  TextColor: #ffffff,
  RenderBold: true,
  HorizontalAlignment: Center
);

Group #DialogRoot {
  Anchor: (Width: 400, Height: 250);
  Background: #1a1a2e(0.98);
  LayoutMode: Top;
  Padding: (Full: 20);

  // Titre
  Label #Title {
    Text: "Confirm Action";
    Anchor: (Height: 32);
    Style: @TitleStyle;
  }

  // Espaceur
  Group { Anchor: (Height: 16); }

  // Message
  Label #Message {
    Text: "Are you sure you want to continue?";
    Anchor: (Height: 60);
    Style: (FontSize: 14, TextColor: #c0c0c0, Wrap: true, HorizontalAlignment: Center);
  }

  // Espaceur flexible
  Group { FlexWeight: 1; }

  // Boutons
  Group #Buttons {
    LayoutMode: Center;
    Anchor: (Height: 50);
    Gap: 16;

    $C.@CancelTextButton #CancelBtn {
      @Text = "Cancel";
      Anchor: (Width: 100, Height: 44);
    }

    $C.@TextButton #ConfirmBtn {
      @Text = "Confirm";
      Anchor: (Width: 100, Height: 44);
    }
  }
}
```
