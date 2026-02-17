# 🐳 Olga Designer & Admin & Backend — Lancement avec Docker

## 🎯 Objectif

Ce guide explique comment démarrer l’ensemble des services de l’application (**Backend, Frontends, Base de données**) à l’aide de **Docker Compose**.

---

## ✅ Prérequis

* **Docker** installé et en cours d’exécution
* **Docker Compose v2** (commande `docker compose`)
  *(ou `docker-compose` si version plus ancienne)*

---

## 📁 Fichiers principaux du projet

* `docker-compose.yml`
* `Dockerfile.admin`
* `Dockerfile.designer`
* Dossier `config/` (fichiers montés dans les conteneurs)

---

## 📁 Fichiers de configuration

> ⚠️ Par défaut, les fichiers de configuration doivent se trouver dans le dossier `./config`.
> Si vous souhaitez les placer ailleurs, il faudra adapter les chemins dans `docker-compose.yml` ou les variables d’environnement correspondantes.

---

### 1️⃣ `config.json`

Ce fichier contient la configuration Firebase et l’URL de l’API pour le développement local.

#### Étapes pour récupérer `firebaseConfig`

1. Connectez-vous à **Firebase**.
2. Aller dans **Paramètres → Paramètres généraux**.
3. Si nécessaire, **ajouter une application**.
4. Copier le contenu de la variable `firebaseConfig`.

#### Exemple de contenu

```json
{
  "EDHA": {
    "label": "Développement Local",
    "firebaseOptions": {
      // coller ici les données récupérées de firebaseConfig
    },
    "apiBaseUrl": "http://localhost:9091/"
  }
}
```

---

### 2️⃣ `apiKey.json`

Ce fichier contient la clé d’API complète pour Firebase ou un service similaire.

#### Exemple de structure

```json
{
  "type": "",
  "project_id": "",
  "private_key_id": "",
  "private_key": "",
  "client_email": "",
  "client_id": "",
  "auth_uri": "",
  "token_uri": "",
  "auth_provider_x509_cert_url": "",
  "client_x509_cert_url": "",
  "universe_domain": ""
}
```

#### Étapes pour remplir ce fichier

1. Générer ou télécharger la **clé de service** depuis Firebase :
   **Paramètres → Comptes de service → Générer une clé**.
2. Copier le contenu JSON fourni par Firebase directement dans `apiKey.json`.

---

## 🌐 Services exposés (ports par défaut)

| Service     | URL / Port                                     |
| ----------- | ---------------------------------------------- |
| MySQL       | 3306 (à modifier)                              |
| phpMyAdmin  | [http://localhost:81](http://localhost:81)     |
| API Backend | [http://localhost:9091](http://localhost:9091) |
| Designer    | [http://localhost:8080](http://localhost:8080) |
| Admin       | [http://localhost:8081](http://localhost:8081) |

Ces ports correspondent aux mappings définis dans `docker-compose.yml`.

---

## 🚀 Démarrer tous les services

## Windows / macOS / Linux

```bash
docker compose up -d --build
```

✔ Construit les images si nécessaire
✔ Démarre tous les services en arrière-plan

---

## 🛑 Arrêter les services

```bash
docker compose down
```

## Supprimer aussi les volumes (⚠ supprime la base de données)

```bash
docker compose down -v
```

---

## 🔄 Rebuild d’un seul service

## Exemple : Designer

```bash
docker compose build designer
docker compose up -d designer
```

Ou en une seule commande :

```bash
docker compose up -d --build designer
```

---

## 📋 Logs & Debug

## Voir les logs de tous les services

```bash
docker compose logs -f
```

### Voir les logs d’un service spécifique

```bash
docker compose logs -f api
```

### Ouvrir un shell dans un conteneur

```bash
docker compose exec api sh
```

---

## ⚙️ Fichiers de configuration importants

| Fichier              | Rôle                              |
| -------------------- | --------------------------------- |
| `config/config.json` | Configuration des frontends       |
| `config/apiKey.json` | Clé Firebase (backend uniquement) |
| `.env.back`          | Variables d’environnement backend |
| `.env.front`         | Variables d’environnement front   |

⚠ **Vérifiez ces fichiers avant de démarrer les services.**

---

## 🔍 Commandes utiles

### Voir les conteneurs actifs

```bash
docker compose ps
```

### Nettoyage complet Docker (⚠ avancé)

```bash
docker system prune --all --volumes
```

---

## 🛠 Dépannage rapide

* Consultez les logs :

  ```bash
  docker compose logs <service>
  ```

* Vérifiez que les ports ne sont pas déjà utilisés sur votre machine.

* Pour réinitialiser la base de données :

  ```bash
  docker compose down -v
  ```

---

## 📌 Architecture simplifiée

```txt
Navigateur
   ↓
Frontend (8080 / 8081)
   ↓
API Backend (9091)
   ↓
MySQL (3306)
```
