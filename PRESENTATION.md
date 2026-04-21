# Restaurant Online

Ouais bon, c'est une app de resto en ligne. User peut regarder le menu, commander des trucs, et nous on peut gérer les commandes. Simple.

## Comment on a fait ça

On s'est mises dessus tous ensemble sur la même machine à la fac. Genre vraiment tous ensemble à partager un clavier/souris mdr. On s'envoyait des bouts de code sur Discord quand fallait valider des trucs ou dire "hey j'ai finit la page menu".

Pas du copy-paste bête. On a vraiment codé ensemble.

---

## Ce qu'on a utilisé de Vue.js

**Directives de base**
- `v-for` pour afficher les listes de plats et commandes
- `v-if / v-else` quand faut décider d'afficher un truc ou pas (panier vide, commande prête...)
- `v-bind` pour les classes dynamiques (le menu actif et tout)

**Composants & props**
- 5 composants qu'on a réutilisé un peu partout
- Les props pour passer les données entre composants
- Genre le composant `Gourmet` qui affiche un plat

**Events custom**
- `@festoyons` quand tu cliques sur "ajouter au panier"
- `@ready` pour marquer une commande prête
- Les autres trucs pour la gestion du panier (quantités, suppressions...)

**Slots**
- Tu peux customiser ce qu'il y a dans un composant avec les slots
- Genre les badges "Nouveau" ou "Le classique"

**Watchers**
- Pour recalculer le total du panier chaque fois que t'ajoutes/supprimes/changes la quantité
- C'est en temps réel

**Lifecycle hooks**
- `mounted()` qui l'affiche "Les moules sont prêtes" dans la console au démarrage (c'est une blague du cours)
- Cleanup des timers quand le composant se casse

**Provide/Inject**
- Au lieu de passer des props partout partout, on partage directement l'état global
- Les pages peuvent accéder au panier et aux commandes sans cascade

**Vue Router**
- 4 pages : accueil, menu, panier, admin
- Navigation normal avec des liens
- Les transitions fluides entre pages

**Animations**
- Des transitions sympa quand tu changes de page
- Le toast qui pop quand tu ajoutes un truc

**Refs et computed**
- ref() pour les variables réactives
- computed() pour les valeurs qu'on calcule
- Tout ça qui se update tout seul

---

## Architecture Générale

```
App.vue (racine)
├── provide('restaurantStore') ← partage l'état global
├── RouterView ← affiche les pages
└── ToastAlert ← notifications globales

Vues (Pages) :
├── HomeView ...................... Accueil avec présentation
├──Structure du projet

Voilà comment on a organisé le truc :

- **App.vue** : conteneur principal, c'est là qu'on partage l'état
- **Pages** : Accueil, Menu, Panier, Admin
  - Accueil: bienvenue + bouton pour aller au menu
  - Menu: les plats avec les boutons pour ajouter
  - Panier: ce qu'il y a dedans, toucher les quantités, valider
  - Admin: les commandes qui traînent, marquer comme prêtes
- **Composants réutilisables** :
  - Gourmet : la carte d'un plat
  - PanierItem : une ligne du panier
  - CommandeItem : une commande en admin
  - ToastAlert : les notifications
- **menu.js** : les 10 plats avec descriptions/prix...*Node.js** (dev server)
Les outils qu'on a utilisé

- Vue 3 avec la Composition API
- Vite pour le dev (super rapide)
- Vue Router pour la navigation
- CSS 3 pour les styles et animations
- Node.js en localonctionnels
- Events custom (`@festoyons`, etc)
- Watchers (deep + immediate)
- Lifecycle hooks (mounted avec log)
- Pre qu'on a fait pour la note

- Directives de base qui fonctionnent
- Composants avec props/slots
- Events custom pour les interactions
- Watchers pour tracker les changements
- Lifecycle hooks avec le truc "les moules sont prêtes" dans la console
- PPour lancer ça

```bash
npm install    # Une seule fois
npm run dev    # Lance l'app sur http://localhost:5173/
npm run build  # Si t'as envie de prod
```

---

## En gros

On a une vraie app qui marche, avec toutes les features demandées. Pas juste du théorique. 

On s'est bien organisé malgré le fait qu'on était tous sur la même machine. Discord a sauvé la mission pour discuter sans crier dessus.

L'app est prête à montrer en classe
Développé sur une machine partagée, débogué sur Discord.
