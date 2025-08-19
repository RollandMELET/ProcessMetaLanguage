
### **Rapport Complet et Définitif sur les Objets et Composants de l'API 360SmartConnect V2**

**Source :** `https://apiv2.360sc.yt/api/docs.jsonopenapi`
**Date de l'analyse :** 19 août 2025

#### **Introduction**

Ce rapport détaille de manière exhaustive l'ensemble des objets, propriétés et composants qu'il est possible de manipuler via l'API V2 de 360SmartConnect. Il inclut une description fonctionnelle de chaque objet, un tableau détaillé de ses propriétés, et le schéma JSON correspondant à sa structure de données.

L'API est structurée en deux parties principales :
1.  **Un modèle de données principal** : Un ensemble de ressources interdépendantes gérables via des opérations standard (CRUD).
2.  **Des endpoints et modules spécialisés** : Des points d'accès à des fonctionnalités spécifiques qui ne sont pas modélisés comme des entités de données classiques.

---

### **Partie 1 : Le Modèle de Données Principal**

#### **1. Objet `Avatar`**
Représente un objet physique ou logique connecté (un "avatar"). C'est l'objet central de l'API.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique de l'avatar. |
| `name` | string | | Nom de l'avatar. |
| `serialNumber` | string | | Numéro de série. |
| `creationDate` | string (date-time) | readOnly | Date de création. |
| `lastUpdate` | string (date-time) | readOnly | Date de la dernière mise à jour. |
| `status` | integer | | Statut de l'avatar. |
| `avatarType` | IRI (string) | Obligatoire | Lien vers le type d'avatar (`AvatarType`). |
| `company` | IRI (string) | | Lien vers la société propriétaire (`Company`). |
| `parent` | IRI (string) | | Lien vers un avatar parent (relation hiérarchique). |
| `children` | array[IRI] | readOnly | Liste des liens vers les avatars enfants. |
| `properties` | array[IRI] | readOnly | Liste des propriétés associées (`AvatarProperty`). |
| `groups` | array[IRI] | | Groupes auxquels l'avatar appartient (`GroupAvatar`). |
| `users` | array[IRI] | | Utilisateurs ayant accès à cet avatar (`User`). |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "name": { "type": "string" },
    "serialNumber": { "type": "string" },
    "creationDate": { "type": "string", "format": "date-time", "readOnly": true },
    "lastUpdate": { "type": "string", "format": "date-time", "readOnly": true },
    "status": { "type": "integer" },
    "avatarType": { "type": "string", "description": "IRI vers une ressource AvatarType" },
    "company": { "type": "string", "description": "IRI vers une ressource Company" },
    "parent": { "type": "string", "description": "IRI vers une ressource Avatar (parent)" },
    "children": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource Avatar (enfant)" },
      "readOnly": true
    },
    "properties": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource AvatarProperty" },
      "readOnly": true
    },
    "groups": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource GroupAvatar" }
    },
    "users": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource User" }
    }
  },
  "required": ["avatarType"]
}
```

---
#### **2. Objet `AvatarType`**
Définit une catégorie ou un modèle d'avatar (ex: "Capteur de température", "Badge NFC").

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique du type d'avatar. |
| `name` | string | Obligatoire | Nom du type d'avatar. |
| `company` | IRI (string) | Obligatoire | Lien vers la société propriétaire de ce type. |
| `metadata` | array[IRI] | readOnly | Liste des types de métadonnées associés (`MetadataAvatarType`). |
| `avatars` | array[IRI] | readOnly | Liste des avatars concrets de ce type. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "name": { "type": "string" },
    "company": { "type": "string", "description": "IRI vers une ressource Company" },
    "metadata": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource MetadataAvatarType" },
      "readOnly": true
    },
    "avatars": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource Avatar" },
      "readOnly": true
    }
  },
  "required": ["name", "company"]
}
```

---
#### **3. Objet `MetadataAvatarType`**
Définit le type d'une métadonnée qui peut être associée à un `AvatarType` (ex: "Couleur", "Poids").

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique. |
| `name` | string | Obligatoire | Nom de la métadonnée (ex: "Couleur"). |
| `type` | string | Obligatoire | Type de donnée attendu (ex: "string", "int", "date"). |
| `mandatory` | boolean | | Indique si cette propriété est obligatoire pour les avatars. |
| `avatarType` | IRI (string) | Obligatoire | Lien vers l'`AvatarType` concerné. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "name": { "type": "string" },
    "type": { "type": "string" },
    "mandatory": { "type": "boolean" },
    "avatarType": { "type": "string", "description": "IRI vers une ressource AvatarType" }
  },
  "required": ["name", "type", "avatarType"]
}
```

---
#### **4. Objet `AvatarProperty`**
Représente la valeur concrète d'une métadonnée pour un avatar spécifique.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique. |
| `value` | string | Obligatoire | La valeur de la propriété (ex: "Bleu"). |
| `avatar` | IRI (string) | Obligatoire | Lien vers l'`Avatar` concerné. |
| `meta` | IRI (string) | Obligatoire | Lien vers la définition de la métadonnée (`MetadataAvatarType`). |
| `creationDate` | string (date-time) | readOnly | Date de création. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "value": { "type": "string" },
    "avatar": { "type": "string", "description": "IRI vers une ressource Avatar" },
    "meta": { "type": "string", "description": "IRI vers une ressource MetadataAvatarType" },
    "creationDate": { "type": "string", "format": "date-time", "readOnly": true }
  },
  "required": ["value", "avatar", "meta"]
}
```

---
#### **5. Objet `User`**
Représente un compte utilisateur.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique. |
| `username` | string | Obligatoire | Nom d'utilisateur. |
| `email` | string | Obligatoire | Adresse email. |
| `roles` | array[string] | | Rôles de l'utilisateur (ex: `ROLE_USER`). |
| `password` | string | writeOnly | Mot de passe (requis à la création). |
| `company` | IRI (string) | | Lien vers la société de l'utilisateur (`Company`). |
| `groups` | array[IRI] | | Groupes auxquels l'utilisateur appartient (`GroupUser`). |
| `enabled` | boolean | | Indique si le compte est actif. |
| `lastLogin` | string (date-time) | readOnly | Date de la dernière connexion. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "username": { "type": "string" },
    "email": { "type": "string", "format": "email" },
    "roles": { "type": "array", "items": { "type": "string" } },
    "password": { "type": "string", "writeOnly": true },
    "company": { "type": "string", "description": "IRI vers une ressource Company" },
    "groups": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource GroupUser" }
    },
    "enabled": { "type": "boolean" },
    "lastLogin": { "type": "string", "format": "date-time", "readOnly": true }
  },
  "required": ["username", "email", "password"]
}
```

---
#### **6. Objet `Company`**
Représente une entité d'entreprise.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique. |
| `name` | string | Obligatoire | Nom de la société. |
| `status` | integer | | Statut de la société. |
| `users` | array[IRI] | readOnly | Liste des utilisateurs de cette société. |
| `avatarTypes` | array[IRI] | readOnly | Liste des types d'avatars appartenant à la société. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "name": { "type": "string" },
    "status": { "type": "integer" },
    "users": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource User" },
      "readOnly": true
    },
    "avatarTypes": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource AvatarType" },
      "readOnly": true
    }
  },
  "required": ["name"]
}
```

---
#### **7. Objets `GroupAvatar` et `GroupUser`**
Permettent de regrouper des avatars ou des utilisateurs. Leur structure est identique.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique du groupe. |
| `name` | string | Obligatoire | Nom du groupe. |
| `avatars` / `users` | array[IRI] | | Liste des membres (avatars ou utilisateurs) du groupe. |

##### Schéma JSON (`GroupAvatar`)
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "name": { "type": "string" },
    "avatars": {
      "type": "array",
      "items": { "type": "string", "description": "IRI vers une ressource Avatar" }
    }
  },
  "required": ["name"]
}
```

---
#### **8. Objet `Media`**
Représente un fichier média téléversé. Le schéma ci-dessous correspond à la réponse de l'API après un téléversement réussi.

| Propriété | Type | Attributs | Description |
| :--- | :--- | :--- | :--- |
| `id` | integer | readOnly | Identifiant unique du média. |
| `file` | string (binary) | writeOnly | Le fichier à téléverser (utilisé pour la requête `POST`). |
| `filePath` | string | readOnly | Le chemin d'accès au fichier sur le serveur. |

##### Schéma JSON
```json
{
  "type": "object",
  "properties": {
    "id": { "type": "integer", "readOnly": true },
    "filePath": { "type": "string", "readOnly": true },
    "file": {
        "type": "string",
        "format": "binary",
        "writeOnly": true,
        "description": "Propriété utilisée uniquement pour la requête POST de création."
    }
  }
}
```

---

### **Partie 2 : Endpoints et Modules Spécialisés**

Cette section liste les composants qui ne suivent pas le modèle de données classique et n'ont donc pas de schéma JSON global. Ils représentent des fonctionnalités spécifiques accessibles via des endpoints dédiés.

*   **`Finger`** : Module lié à l'identification ou la traçabilité.
*   **`History`** : Endpoint pour consulter l'historique des actions sur les processus.
*   **`RunnerTemplate`** : Système d'exécution de tâches ou de processus basés sur des modèles.
*   **`MC`** : Endpoint spécifique dont la nature exacte n'est pas déterminable sans documentation fonctionnelle.
*   **`Yourls`** : Intégration avec le service de raccourcissement d'URL Yourls.
*   **`CapremibHS2`** : Endpoint métier spécifique, probablement une intégration ou une fonctionnalité dédiée.