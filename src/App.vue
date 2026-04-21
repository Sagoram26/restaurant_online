<script setup>
import { computed, onMounted, onUnmounted, provide, ref } from 'vue'
import { RouterLink, RouterView, useRoute } from 'vue-router'
import ToastAlert from './components/ToastAlert.vue'

const route = useRoute()

const cart = ref([])
const orders = ref([])

const navLinks = [
  { to: '/', label: 'Accueil' },
  { to: '/menu', label: 'Menu' },
  { to: '/panier', label: 'Panier' },
  { to: '/admin', label: 'Admin' },
]

const toastMessage = ref('')
const toastVisible = ref(false)
let toastTimer = null

const showToast = (message) => {
  toastMessage.value = message
  toastVisible.value = true

  if (toastTimer) {
    clearTimeout(toastTimer)
  }

  toastTimer = setTimeout(() => {
    toastVisible.value = false
  }, 2400)
}

const addDishToCart = (dish) => {
  const existing = cart.value.find((item) => item.id === dish.id)

  if (existing) {
    existing.quantity += 1
    return
  }

  cart.value.push({ ...dish, quantity: 1 })
}

const updateCartItemQuantity = (dishId, quantity) => {
  const item = cart.value.find((dish) => dish.id === dishId)
  if (!item) {
    return
  }

  item.quantity = Math.max(1, Number(quantity) || 1)
}

const removeCartItem = (dishId) => {
  cart.value = cart.value.filter((item) => item.id !== dishId)
}

const placeOrder = (customerName) => {
  if (!cart.value.length) {
    return null
  }

  const total = cart.value.reduce((sum, item) => {
    return sum + item.price * item.quantity
  }, 0)

  const newOrder = {
    id: Date.now(),
    customerName,
    status: 'en-cours',
    createdAt: new Date().toLocaleTimeString('fr-FR', {
      hour: '2-digit',
      minute: '2-digit',
    }),
    items: cart.value.map((item) => ({ ...item })),
    total,
  }

  orders.value.unshift(newOrder)
  cart.value = []
  return newOrder
}

const markOrderReady = (orderId) => {
  const order = orders.value.find((item) => item.id === orderId)
  if (!order) {
    return
  }

  order.status = 'prete'
  showToast(`Commande #${order.id} prete a servir.`)
}

const cartItemsCount = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.quantity, 0)
})

provide('restaurantStore', {
  cart,
  orders,
  addDishToCart,
  updateCartItemQuantity,
  removeCartItem,
  placeOrder,
  markOrderReady,
  showToast,
})

onMounted(() => {
  console.log('Les moules sont prêtes')
})

onUnmounted(() => {
  if (toastTimer) {
    clearTimeout(toastTimer)
  }
})
</script>

<template>
  <div class="app-shell">
    <header class="app-header">
      <div>
        <p class="kicker">Restaurant en ligne</p>
        <h1>Le Quai Gourmet</h1>
      </div>

      <nav class="main-nav">
        <RouterLink
          v-for="link in navLinks"
          :key="link.to"
          :to="link.to"
          v-bind:class="{ active: route.path === link.to }"
        >
          {{ link.label }}
        </RouterLink>
      </nav>
    </header>

    <main class="app-main">
      <RouterView v-slot="{ Component }">
        <Transition name="page-slide" mode="out-in">
          <component :is="Component" />
        </Transition>
      </RouterView>
    </main>

    <RouterLink v-if="cartItemsCount > 0" class="floating-cart" to="/panier">
      Panier actif ({{ cartItemsCount }})
    </RouterLink>

    <Transition name="toast-pop">
      <ToastAlert v-if="toastVisible" :message="toastMessage" />
    </Transition>
  </div>
</template>
