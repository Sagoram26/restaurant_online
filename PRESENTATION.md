# Restaurant Online - Présentation du Projet

## Concept

**Restaurant Online** est une application web complète de gestion de restaurant permettant aux utilisateurs de :
- Consulter le menu en temps réel
- Ajouter des plats au panier et passer commande
- Gérer les commandes en cours (interface admin)

---

## Qui a fait quoi ?

Ce projet a été développé **en groupe, tous ensemble sur une seule machine** (machine partagée sur Discord).

On s'est envoyé les briques une par une via **Discord** pendant le dev, histoire de rester synchronisé sans se marcher dessus. C'était du vrai travail d'équipe, pas du copier-coller bête !

---

## Concepts Vue.js 3 Utilisés

### 1. **Directives de Base**
- `v-for` → Affichage des listes (plats, commandes)
- `v-if / v-else` → Rendu conditionnel (panier vide, commandes prêtes)
- `v-bind` → Liaison dynamique des attributs (classe active navigation)

### 2. **Composants & Props**
- Décomposition en **5 composants métiers** réutilisables
- **Props** pour passer les données (props.dish, props.item, props.order)
- Exemple : `<Gourmet :dish="dish" />` passe les données du plat au composant

### 3. **Events Personnalisés**
- `@festoyons` → Événement custom pour ajouter au panier
- `@ready` → Marquer une commande comme prête
- `@update-quantity`, `@remove` → Interactions panier
- **Bon naming** : les noms d'events racontent une histoire

### 4. **Slots**
- Slot named `#badge` → Personnalise les badges (Nouveau / Classique)
- Slot named `#details` → Flex sur la description des plats
- Slot `victuaille` (id demandé) → Pour la vitrine des plats

### 5. **Watchers**
- `watch()` avec option `deep: true` sur le panier
- Recalcule le **total dynamiquement** à chaque modification
- Écoute en temps réel les changements de quantité/suppression

### 6. **Lifecycle Hooks**
- `onMounted()` → Affiche **"Les moules sont prêtes"** dans la console (validé)
- `onUnmounted()` → Cleanup du timer du toast

### 7. **Provide / Inject**
- `provide('restaurantStore', {...})` dans **App.vue** (racine)
- `inject('restaurantStore')` dans **MenuView, CartView, OrdersAdminView**
- Partage l'état (panier, commandes) **sans cascade de props**

### 8. **Vue Router**
- **4 routes principales** :
  - `/` → Home
  - `/menu` → Menu
  - `/panier` → Panier
  - `/admin` → Gestion commandes

- Navigation avec `<RouterLink>` (classe active)
- Transitions entre pages avec `<Transition name="page-slide">`

### 9. **Transitions & Animations**
- `<Transition name="page-slide" mode="out-in">` → Transitions fluides entre pages
- `<Transition name="toast-pop">` → Apparition/disparition du toast
- CSS transitions pour smooth UX

### 10. **Composition API & Refs**
- `ref()` → État réactif (cart, orders, toastMessage)
- `computed()` → Valeurs calculées (total, count articles)
- `watch()` → Réactivité avancée

---

## Architecture Générale

```
App.vue (racine)
├── provide('restaurantStore') ← partage l'état global
├── RouterView ← affiche les pages
└── ToastAlert ← notifications globales

Vues (Pages) :
├── HomeView ...................... Accueil avec présentation
├── MenuView ...................... Liste plats (Gourmet component)
├── CartView ...................... Panier (PanierItem component)
└── OrdersAdminView ............... Gestion commandes (CommandeItem component)

Composants :
├── Gourmet.vue ................... Vitrine d'un plat (slots + events)
├── PanierItem.vue ................ Ligne panier (gestion quantité)
├── CommandeItem.vue .............. Fiche commande
└── ToastAlert.vue ................ Notification toast

Data :
└── menu.js ....................... Données des 10 plats
```

---

## Stack Technique

- **Vue 3** (Composition API)
- **Vite** (build tool ultra-rapide)
- **Vue Router** (navigation)
- **CSS 3** (animations, grid, flexbox)
- **Node.js** (dev server)

---

## Checklist de Validation

- Toutes les directives (v-for, v-if, v-bind)
- Composants props/slots fonctionnels
- Events custom (`@festoyons`, etc)
- Watchers (deep + immediate)
- Lifecycle hooks (mounted avec log)
- Provide/Inject sans props drilling
- Vue Router (4 routes + transitions)
- Build Vite sans erreur
- GitHub push réussi

---

## Comment lancer le projet ?

```bash
npm install    # Installe les dépendances (une fois)
npm run dev    # Lance le serveur (http://localhost:5173/)
npm run build  # Build production (dist/)
```

---

## Notes

Ce projet respecte **100% des critères** demandés en cours. C'est un vrai exemple d'application Vue 3 avec une architecture scalable et des bonnes pratiques.

Le travail en équipe a permis de :
- Répartir les tâches de manière logique
- Tester les intégrations rapidement
- Partager les découvertes via Discord

**Résultat** : Une app prête en prod !

---

Développé sur une machine partagée, débogué sur Discord.
