# Restaurant en Ligne

Application Vue.js pour gérer un restaurant en ligne.

## Installation

```bash
npm install
npm run dev
```

App sur http://localhost:5173

## Structure

- **HomeView.vue** : Page d'accueil avec description restaurant
- **MenuView.vue** : Liste des plats avec button "Ajouter au panier"
- **CartView.vue** : Panier avec modification quantités et total
- **AdminView.vue** : Gestion des commandes
- **Gourmet.vue** : Composant réutilisable pour un plat

## Fonctionnalités

- Page accueil avec bouton accès menu
- Menu avec liste plats
- Panier avec +/- quantité et suppression
- Finaliser commande avec nom client
- Admin pour voir commandes et marquer comme "prête"
- Toast notifications

## Concepts Vue.js

- v-for, v-if, v-bind
- Props et Events
- Slots
- Watchers et Computed properties
- Lifecycle hooks (onMounted)
- Provide/Inject
- Vue Router

## Build

```bash
npm run build
```
