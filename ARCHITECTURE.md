# 🏗️ Architecture CQRS + Event Sourcing

## 1. Vue globale
```
Client HTTP
     │
     ▼
Express Router (accountRoutes.js)
     │
     ├──► COMMANDS (écriture)        ├──► QUERIES (lecture)
     │    accountCommands.js         │    accountQueries.js
     │         │                     │         │
     │         ▼                     │         ▼
     │    EventStore.js              │    EventStore.js
     │         │                     │         │
     └─────────┴─────────────────────┘         │
                         │                     │
                         ▼                     ▼
                    PostgreSQL (table events)
```

## 2. Flux d'une commande (écriture)
```
1. Client envoie POST /api/accounts/:id/deposit
2. accountRoutes.js reçoit la requête
3. accountCommands.js valide les données
4. EventStore.js charge les événements existants
5. Account.js rehydrate l'état actuel
6. EventStore.js sauvegarde le nouvel événement
7. Réponse renvoyée au client
```

## 3. Flux d'une requête (lecture)
```
1. Client envoie GET /api/accounts/:id/balance
2. accountRoutes.js reçoit la requête
3. accountQueries.js charge les événements
4. Account.js rehydrate et calcule le solde
5. Réponse renvoyée au client
```

## 4. Structure de la table events
```
┌─────────────────────────────────────────────────────┐
│                    TABLE events                      │
├────┬──────────────┬──────────────┬─────────┬────────┤
│ id │ aggregate_id │ event_type   │ payload │version │
├────┼──────────────┼──────────────┼─────────┼────────┤
│  1 │ acc-uuid-1   │ACCOUNT_CREATED│{...}   │   1    │
│  2 │ acc-uuid-1   │MONEY_DEPOSITED│{...}   │   2    │
│  3 │ acc-uuid-1   │MONEY_WITHDRAWN│{...}   │   3    │
└────┴──────────────┴──────────────┴─────────┴────────┘
```

## 5. Choix techniques

| Technologie | Raison |
|-------------|--------|
| Node.js + Express | Léger, rapide, adapté aux APIs REST |
| PostgreSQL | Fiable, supporte JSONB pour les payloads |
| Jest | Framework de tests simple et puissant |
| Docker | Facilite le déploiement et la reproductibilité |
| GitHub Actions | CI/CD gratuit et intégré à GitHub |