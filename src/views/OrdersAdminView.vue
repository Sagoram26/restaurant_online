<script setup>
import { computed, inject } from 'vue'
import CommandeItem from '../components/CommandeItem.vue'

const store = inject('restaurantStore')

const totalOrders = computed(() => store.orders.value.length)
const readyOrders = computed(() => {
  return store.orders.value.filter((order) => order.status === 'prete').length
})
</script>

<template>
  <section class="view-shell">
    <div class="view-title">
      <h2>Gestion des commandes</h2>
      <p>Vue admin des commandes cuisine.</p>
    </div>

    <div class="admin-stats">
      <p>Total: {{ totalOrders }}</p>
      <p>Pretes: {{ readyOrders }}</p>
      <p>En cours: {{ totalOrders - readyOrders }}</p>
    </div>

    <p v-if="totalOrders === 0" class="empty-state">Aucune commande en cours.</p>

    <div v-else class="orders-grid">
      <CommandeItem
        v-for="order in store.orders.value"
        :key="order.id"
        :order="order"
        @ready="store.markOrderReady"
      />
    </div>
  </section>
</template>
