# Guide de Configuration pour les Administrateurs 🛠️

Ce guide détaille comment personnaliser les métiers du serveur FReZ Hytale.

## 📂 Emplacement des Fichiers
Les métiers sont chargés dynamiquement depuis :
`run/mods/com.jjeanniard.plugins_FReZ-Hytale-mod-Professions/professions/`

Toute modification nécessite l'exécution de la commande `/profession reload` en jeu ou en console.

---

## 🏗️ Structure d'un fichier métier (`.json`)

### Exemple complet : `MINEUR.json`
```json
{
  "Id": "MINEUR",
  "DisplayName": "Mineur de l'Ombre",
  "TalentMultiplier": 1.2,
  "Statuses": [
    {
      "Id": "apprenti",
      "DisplayName": "Apprenti Mineur",
      "Order": 1,
      "XpRequiredPerLevel": 1000,
      "Levels": [
        { 
          "Level": 1, 
          "Attributes": { "harvest_speed": 1.1, "xp_bonus": 1.0 }, 
          "Permissions": ["frez.mineur.basic"] 
        },
        { 
          "Level": 30, 
          "Attributes": { "harvest_speed": 1.5, "drop_multiplier": 1.1 }, 
          "Permissions": ["frez.mineur.iron"] 
        }
      ]
    }
  ],
  "XpRules": [
    { "Action": "BLOCK_BREAK", "TargetId": "group:Ores", "Xp": 25, "Money": 5.0 },
    { "Action": "BLOCK_BREAK", "TargetId": "stone", "Xp": 2, "Money": 0.1 }
  ],
  "Talents": [
    {
      "Id": "mineur_vision",
      "ProfessionId": "MINEUR",
      "DisplayName": "Vision Nocturne",
      "Description": "Vous voyez mieux dans le noir complet.",
      "Cost": 3,
      "PrerequisiteIds": [],
      "PermissionNode": "hytale.effect.night_vision"
    }
  ]
}
```

---

## 📈 Système de Gain d'XP (`XpRules`)

| Action | Description | Cible typique (`TargetId`) |
| :--- | :--- | :--- |
| `BLOCK_BREAK` | Destruction d'un bloc | `stone`, `group:Wood`, `Ore_Iron` |
| `BLOCK_PLACE` | Pose d'un bloc | `cobblestone`, `group:Dirt`, `Seed_Oak` |
| `BLOCK_USE` | Interaction (récolte/outil) | `group:Crop`, `Watering_Can`, `CookingPot` |
| `ENTITY_KILL` | Tuer une créature | `spider`, `trork`, `VoidEye` |
| `PICKUP` | Ramasser un item | `group:Fish`, `Food_Fish_Raw_Rare` |
| `CRAFT` | Fabriquer un item | `bread`, `Potion_Health`, `Ingot_Iron` |
| `ITEM_USE` | Consommer un item | `Potion_Health`, `Scroll` |
| `DISCOVER_ZONE`| Découvrir une zone | `Zone_Forest`, `Dungeon_Skeleton` |

### 💡 Astuce "Loose Matching"
Le plugin utilise un système de correspondance intelligente :
1. **Namespace ignoré** : `hytale:log_oak` peut être écrit simplement `log_oak`.
2. **Groupes** : Utilisez le préfixe `group:` pour cibler une catégorie entière définie par Hytale (ex: `group:Wood`, `group:Ores`).
3. **Mots-clés** : Si vous mettez `Potion`, cela donnera de l'XP pour toutes les potions (`Potion_Health`, `Potion_Mana`, etc.).

---

## 🏆 Grades et Niveaux (`Statuses`)

Chaque grade (Apprenti, Compagnon...) possède **30 niveaux**.
- `XpRequiredPerLevel` : C'est le montant fixe pour passer de 1 à 2, de 2 à 3, etc. au sein de ce grade.
- `Levels` : Vous ne définissez que les paliers importants (ex: 1, 15, 30). Les attributs du dernier palier atteint restent actifs.

### Attributs Disponibles :
- `harvest_speed` : `1.5` = +50% de vitesse.
- `xp_bonus` : `1.2` = +20% d'XP gagnée.
- `money_bonus` : `2.0` = double les gains d'argent.
- `damage_dealt` : `1.1` = +10% de dégâts.
- `drop_multiplier` : `1.5` = 50% de chance de double loot (si implémenté).

---

## 🎭 Arbre de Talents (`Talents`)

Les points de talent sont gagnés dynamiquement selon la formule :
`Points = (Ordre_du_Grade + floor(Niveau / 10)) * TalentMultiplier`

- `Cost` : Coût en points.
- `PrerequisiteIds` : IDs des talents à posséder avant (ex: `["talent_1"]`).
- `PermissionNode` : Noeud LuckPerms donné au joueur lors de l'achat.
