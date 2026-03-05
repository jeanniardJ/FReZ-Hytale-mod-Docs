# 📋 Liste des Événements Serveur

Ce document sert de référence rapide pour les événements les plus courants disponibles côté serveur. Pour apprendre à utiliser ces événements dans votre code, veuillez consulter le guide principal : **[Le Système d'Événements](general/event_system.md)**.

---

## 1. Événements du Joueur (`com.hypixel.hytale.server.core.event.events.player`)

Ces événements sont déclenchés par des actions directes ou des changements d'état liés à un joueur.

*   `PlayerConnectEvent` : Déclenché lorsqu'un joueur se connecte au serveur.
*   `PlayerDisconnectEvent` : Déclenché lorsqu'un joueur se déconnecte.
*   `PlayerReadyEvent` : Déclenché lorsque le client du joueur a fini de charger le monde et est prêt à jouer.
*   `PlayerChatEvent` : Déclenché lorsqu'un joueur envoie un message dans le chat. (Annulable)
*   `PlayerInteractEvent` : Déclenché lorsqu'un joueur interagit avec une entité ou un bloc dans le monde.
*   `PlayerCraftEvent` : Déclenché lorsqu'un joueur effectue un artisanat (craft).
*   `AddPlayerToWorldEvent` : Lié au processus d'ajout d'un joueur à un monde spécifique.
*   `DrainPlayerFromWorldEvent` : Lié au processus de retrait d'un joueur d'un monde.
*   `PlayerMouseButtonEvent` : Déclenché par une action de clic de la souris du joueur.
*   `PlayerMouseMotionEvent` : Déclenché par un mouvement de la souris du joueur.
*   `PlayerSetupConnectEvent` / `PlayerSetupDisconnectEvent` : Étapes techniques du cycle de vie de la connexion.

---

## 2. Événements du Monde et des Blocs (`...events.ecs`)

Ces événements sont liés aux interactions avec le monde physique du jeu. Ils sont souvent dans le package `ecs` car ils sont gérés par les systèmes ECS.

*   `PlaceBlockEvent` : Déclenché lorsqu'un bloc est posé. (Annulable)
*   `BreakBlockEvent` : Déclenché lorsqu'un bloc est cassé. (Annulable)
*   `DamageBlockEvent` : Déclenché lorsqu'un bloc subit des dégâts (avant d'être potentiellement cassé). (Annulable)
*   `UseBlockEvent` : Déclenché lorsqu'un joueur fait un clic droit sur un bloc. (Annulable)
*   `DiscoverZoneEvent` : Déclenché lorsqu'un joueur découvre une nouvelle zone.
*   `DropItemEvent` : Déclenché lorsqu'un objet est lâché dans le monde.
*   `CraftRecipeEvent` : Déclenché lors de la fabrication d'une recette.
*   `SwitchActiveSlotEvent` : Déclenché lorsque le joueur change l'objet dans sa barre d'accès rapide.
*   `InteractivelyPickupItemEvent` : Déclenché lorsqu'un joueur ramasse un objet.

---

## 3. Événements des Entités (`...events.entity`)

Ces événements concernent les entités en général (monstres, animaux, objets au sol, etc.).

*   `EntityRemoveEvent` : Déclenché lorsqu'une entité est retirée du monde.
*   `LivingEntityInventoryChangeEvent` : Déclenché lorsque l'inventaire d'une entité vivante (PNJ, monstre) change.
*   `LivingEntityUseBlockEvent` : Déclenché lorsqu'une entité vivante interagit avec un bloc.
*   `EntityEvent` : Classe de base pour les autres événements d'entité.

---

## 4. Événements du Serveur (`...event.events`)

Ces événements sont liés au cycle de vie du serveur lui-même.

*   `BootEvent` : Déclenché au tout début du démarrage du serveur.
*   `PrepareUniverseEvent` : Déclenché lors de la préparation des mondes (l'univers).
*   `ShutdownEvent` : Déclenché juste avant l'arrêt complet du serveur.
