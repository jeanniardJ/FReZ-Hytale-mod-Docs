# Authentication API

Le système d'authentification gère l'identité des joueurs et le stockage des identifiants.

## PlayerAuthentication

Représente les données d'authentification d'un joueur connecté.

### Constantes

* `MAX_REFERRAL_DATA_SIZE` : 4096 octets (Taille maximale des données de parrainage).

### Propriétés

* **UUID** : Identifiant unique du joueur (Requis).
* **Username** : Nom d'utilisateur (Requis).
* **ReferralData** : Données de parrainage optionnelles (`byte[]`).
* **ReferralSource** : Adresse source du parrainage (`HostAddress`).

### Méthodes Principales

```java
import com.hypixel.hytale.server.core.auth.PlayerAuthentication;

PlayerAuthentication auth = ...;

// Récupération (Lève une exception si non défini)
String name = auth.getUsername();
UUID uuid = auth.getUuid();

// Gestion du parrainage
byte[] refData = auth.getReferralData();
HostAddress refSource = auth.getReferralSource();

// Modification
auth.setReferralData(new byte[...]); // Throws IllegalArgumentException si > 4096 bytes
```

## AuthCredentialStoreProvider

Interface pour les fournisseurs de stockage d'identifiants.

### Implémentations

* **MemoryAuthCredentialStoreProvider** : Stockage en mémoire (non persistant).
    * **ID**: `"Memory"`
    * **Codec**: `MemoryAuthCredentialStoreProvider.CODEC`

```java
import com.hypixel.hytale.server.core.auth.MemoryAuthCredentialStoreProvider;

// Création d'un store en mémoire
IAuthCredentialStore store = new MemoryAuthCredentialStoreProvider().createStore();
```
