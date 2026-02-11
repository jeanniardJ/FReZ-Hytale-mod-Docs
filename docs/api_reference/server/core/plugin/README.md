# Référence API du Système de Plugins du Cœur du Serveur

Cette section fournit une documentation API détaillée pour les composants clés de la gestion des plugins du serveur Hytale. Ces classes sont fondamentales pour développer et gérer les plugins (vos mods) qui étendent les fonctionnalités du serveur.

## Classes

-   [`PluginManager`](./PluginManager.md) : Gère le cycle de vie de tous les plugins sur le serveur (chargement, démarrage, arrêt, etc.).
-   [`PluginBase`](./PluginBase.md) : La classe de base abstraite pour tous les plugins Hytale. Elle fournit des fonctionnalités communes et un accès aux divers registres du serveur.
-   [`JavaPlugin`](./JavaPlugin.md) : Une classe abstraite concrète qui étend `PluginBase`, spécifiquement conçue pour les plugins écrits en Java. C'est la classe que la plupart de vos mods étendront.
