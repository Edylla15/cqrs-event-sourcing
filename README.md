# 🏦 CQRS Event Sourcing - Gestion de compte bancaire

## 📋 Description
Mini-projet illustrant les patterns **CQRS** (Command Query Responsibility Segregation) et **Event Sourcing** appliqués à une gestion de compte bancaire simple.

## 🏗️ Architecture

### CQRS
- **Commands** (écriture) : créer, déposer, retirer, fermer
- **Queries** (lecture) : solde, historique, liste des comptes

### Event Sourcing
Chaque opération est sauvegardée comme un événement immuable dans PostgreSQL. L'état du compte est reconstruit en rejouant les événements.

## 🚀 Installation et lancement

### Prérequis
- Node.js 18+
- PostgreSQL 16+ OU Docker

### Sans Docker
```bash
# 1. Cloner le projet
git clone <url-du-repo>
cd cqrs-event-sourcing

# 2. Installer les dépendances
npm install

# 3. Créer la base de données PostgreSQL
psql -U postgres -c "CREATE DATABASE cqrs_bank;"
psql -U postgres -d cqrs_bank -f init.sql

# 4. Lancer le serveur
node index.js
```

### Avec Docker
```bash
docker-compose up --build
```

## 🔗 Endpoints API

### Commands (écriture)
| Méthode | URL | Description |
|---------|-----|-------------|
| POST | /api/accounts | Créer un compte |
| POST | /api/accounts/:id/deposit | Déposer de l'argent |
| POST | /api/accounts/:id/withdraw | Retirer de l'argent |
| POST | /api/accounts/:id/close | Fermer un compte |

### Queries (lecture)
| Méthode | URL | Description |
|---------|-----|-------------|
| GET | /api/accounts | Lister tous les comptes |
| GET | /api/accounts/:id/balance | Voir le solde |
| GET | /api/accounts/:id/history | Voir l'historique |

## 🧪 Tests
```bash
npm test
```

## 🐳 Docker
```bash
# Lancer avec Docker Compose
docker-compose up --build

# Arrêter
docker-compose down
```

## 📁 Structure du projet
```
cqrs-event-sourcing/
├── src/
│   ├── aggregates/
│   │   └── Account.js        # Agrégat compte bancaire
│   ├── commands/
│   │   └── accountCommands.js # Commandes (écriture)
│   ├── eventStore/
│   │   └── EventStore.js     # Stockage des événements
│   ├── queries/
│   │   └── accountQueries.js # Requêtes (lecture)
│   ├── routes/
│   │   └── accountRoutes.js  # Routes Express
│   └── db.js                 # Connexion PostgreSQL
├── tests/
│   └── account.test.js       # Tests Jest
├── .github/
│   └── workflows/
│       └── ci.yml            # CI/CD GitHub Actions
├── Dockerfile
├── docker-compose.yml
├── init.sql
└── README.md
```

## 🔧 Technologies utilisées
- **Node.js** + **Express** — serveur API
- **PostgreSQL** — base de données Event Store
- **Jest** — tests automatisés
- **Docker** — conteneurisation
- **GitHub Actions** — CI/CD