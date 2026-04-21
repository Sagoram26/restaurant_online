<script setup>
import { computed, inject, ref, watch } from 'vue'
import PanierItem from '../components/PanierItem.vue'

const store = inject('restaurantStore')
const customerName = ref('')
const cartTotal = ref(0)

watch(
  () => store.cart.value,
  (items) => {
    cartTotal.value = items.reduce((sum, item) => sum + item.price * item.quantity, 0)
  },
  {
    deep: true,
    immediate: true,
  },
)

const hasItems = computed(() => store.cart.value.length > 0)

const updateQuantity = (payload) => {
  store.updateCartItemQuantity(payload.id, payload.quantity)
}

const removeDish = (dishId) => {
  store.removeCartItem(dishId)
}

const formatPrice = (price) => {
  return new Intl.NumberFormat('fr-FR', {
    style: 'currency',
    currency: 'EUR',
  }).format(price)
}

const finalizeOrder = () => {
  if (!hasItems.value) {
    return
  }

  const cleanName = customerName.value.trim()
  if (!cleanName) {
    store.showToast('Merci d indiquer le nom du client.')
    return
  }

  store.placeOrder(cleanName)
  customerName.value = ''
  store.showToast(`Commande envoyee pour ${cleanName}.`)
}
</script>

<template>
  <section class="view-shell">
    <div class="view-title">
      <h2>Panier</h2>
      <p>Modifie les quantites puis finalise la commande.</p>
    </div>

    <p v-if="!hasItems" class="empty-state">Le panier est vide. Retourne au menu pour festoyer.</p>

    <div v-else class="cart-layout">
      <div class="cart-items">
        <PanierItem
          v-for="item in store.cart.value"
          :key="item.id"
          :item="item"
          @update-quantity="updateQuantity"
          @remove="removeDish"
        />
      </div>

      <aside class="cart-summary">
        <label for="customerName">Nom du client</label>
        <input id="customerName" v-model="customerName" type="text" placeholder="Ex: Nora" />

        <p>
          Total actuel: <strong>{{ formatPrice(cartTotal) }}</strong>
        </p>

        <button class="cta-main" type="button" @click="finalizeOrder">Finaliser la commande</button>
      </aside>
    </div>
  </section>
</template>
