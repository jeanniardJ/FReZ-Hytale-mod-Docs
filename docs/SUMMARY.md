# Table des Matières

* [Introduction](README.md)
* [Architecture & Règles](1_ARCHITECTURE.md)
* [Configuration & Codecs](3_CONFIGURATION.md)

---

## Référence API

* **Concepts Généraux**
    * [Le Système d'Événements](api_reference/general/event_system.md)
    * [Journalisation (Logging)](api_reference/general/logging.md)
    * [Permissions](api_reference/general/permissions.md)
    * [Authentification](api_reference/general/authentication.md)

* **Serveur & Monde**
    * [API du Monde (World & Universe)](api_reference/world/world_api.md)
    * [Configuration du Monde](api_reference/world/world_config.md)
    * [Suivi de la Carte](api_reference/world/world_map_tracker.md)
    * [Présentation du Coeur du Serveur](api_reference/server/core/README.md)

* **Entités & Joueur**
    * [API des Entités (Entity & LivingEntity)](api_reference/server/core/entity/Entity.md)
    * [API du Joueur (Player)](api_reference/entity/player_api.md)
    * [BlockEntity](api_reference/entity/BlockEntity.md)

* **Commandes**
    * [Le Système de Commandes](api_reference/command/command_system.md)
    * [Types d'Arguments](api_reference/command/ArgTypes.md)

* **Événements**
    * [Liste des Événements Serveur](api_reference/event/server_events_list.md)
    * [Ancienne Doc des Listeners](api_reference/event/listeners.md)

* **Interface Utilisateur (UI)**
    * [UI Personnalisée](api_reference/ui/custom_ui.md)
    * [Messages Formatés](api_reference/ui/message.md)
    * [Titres à l'Écran](api_reference/ui/titles.md)
    * [Dépannage UI](api_reference/ui/troubleshooting.md)

* **Utilitaires**
    * [Correspondance de Noms](api_reference/util/name_matching.md)
    * [Utilitaire de Validation](api_reference/util/validate_util.md)
    * [Correspondance Wildcard](api_reference/util/wildcard_match.md)

---

## Mécanismes Internes

* [Options de Lancement](internals/launch_options.md)
* [Modules Principaux](internals/core_modules.md)
* [Configuration du Serveur](internals/server_config.md)
* [Système d'Arrêt](internals/shutdown_system.md)
* [Moteur de Stockage](internals/storage_engine.md)
* [Sneaky Throw](internals/sneaky_throw.md)
