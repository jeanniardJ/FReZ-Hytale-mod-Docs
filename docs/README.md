> **Note de Navigation :** Pour une table des matières complète et structurée, veuillez consulter le fichier [**SUMMARY.md**](SUMMARY.md).

# Bienvenue dans la Documentation du Projet

Cette documentation sert de "Knowledge Base" (base de connaissances) pour le développement de plugins Hytale. Elle est
conçue pour être claire, concise et accessible, même pour un développeur junior.

N'hésitez pas à l'explorer et à la compléter !

---

## 🚀 Principes Fondamentaux

* [Architecture & Règles de Développement](1_ARCHITECTURE.md)
* [Configuration & Codecs (JSON)](3_CONFIGURATION.md)

## 🤖 Intégration IA

Si vous souhaitez utiliser ce projet comme référence pour l'écriture de code avec l'intelligence artificielle, vous pouvez le télécharger et spécifier l'emplacement du fichier `com/hypixel/hytale` dans vos invites afin que l'agent puisse recevoir des informations sur le projet.

## 📚 Référence API

Cette section fournit la documentation technique des différentes APIs disponibles pour le développement de mods Hytale.

*   **API du Cœur du Serveur**
    *   [Présentation Générale](./api_reference/server/core/README.md)
*   **API des Plugins du Cœur du Serveur**
    *   [Présentation Générale](./api_reference/server/core/plugin/README.md)
*   **API du Système de Commandes du Cœur du Serveur**
    *   [Présentation Générale](./api_reference/server/core/command/system/README.md)
*   **API du Système d'Événements**
    *   [Présentation Générale](./api_reference/event/README.md)
*   **PNJ (Personnages Non Joueurs)**
    *   [Présentation Générale](./api_reference/server/npc/README.md)

*   **Apparition (Spawning)**
    *   [Présentation Générale](./api_reference/server/spawning/README.md)

*   **Composants (ECS)**
    *   [Présentation Générale](./api_reference/component/README.md)

*   **Général**
    *   [Authentification](api_reference/general/authentication.md)
    *   [HytaleServer (Déprécié - Voir "API du Cœur du Serveur")](./api_reference/server/core/README.md)
    *   [Journalisation (Logging)](api_reference/general/logging.md)
    *   [Permissions](api_reference/general/permissions.md)
    *   [Univers (Déprécié - Voir "API du Cœur du Serveur")](./api_reference/world/world_api.md)
    *   [API Core du Serveur Hytale (EN)](api_reference/server_core_api/HYTALE_CORE_API.md)
    *   [API Core du Serveur Hytale (TR)](api_reference/server_core_api/HYTALE_CORE_API_TR.md)
*   **Commandes (Général)**
    *   [Types d'Arguments](api_reference/command/ArgTypes.md)
    *   [Argument Requis](api_reference/command/RequiredArg.md)
*   **Entités (Joueur & PlayerRef)**
    *   [Présentation Générale des Entités](./api_reference/entity/README.md)
*   **Événements (Général)**
    *   [Écouteurs d'Événements](api_reference/event/listeners.md)
*   **Interface Utilisateur (UI)**
    *   [Présentation Générale & Guides](./api_reference/ui/README.md)
*   **Monde**
    *   [Monde (World)](api_reference/world/world.md)
    *   [Configuration du Monde](api_reference/world/world_config.md)
    *   [Suivi de la Carte du Monde](api_reference/world/world_map_tracker.md)
*   **Utilitaires**
    *   [Correspondance de Noms](api_reference/util/name_matching.md)
    *   [Utilitaire de Validation](api_reference/util/validate_util.md)
    *   [Correspondance par Caractères Génériques (WildcardMatch)](api_reference/util/wildcard_match.md)

## 👻 Mécanismes Internes (Avancé)

* [Options de Lancement](internals/launch_options.md)
* [Modules Principaux](internals/core_modules.md)
* [Configuration du Serveur](internals/server_config.md)
* [Système d'Arrêt](internals/shutdown_system.md)
* [Sneaky Throw](internals/sneaky_throw.md)
* [Moteur de Stockage](internals/storage_engine.md)

---

## 🤖 Meta Documentation

* [Directives pour l'Assistant IA](../AGENTS.md)
* [Règles du Projet](../RULES.md)
