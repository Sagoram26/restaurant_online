<script setup>
const props = defineProps({
  order: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['ready'])

const formatPrice = (price) => {
  return new Intl.NumberFormat('fr-FR', {
    style: 'currency',
    currency: 'EUR',
  }).format(price)
}
</script>

<template>
  <article class="order-card" :class="{ 'order-card--ready': props.order.status === 'prete' }">
    <header>
      <h3>{{ props.order.customerName }}</h3>
      <p>
        Commande #{{ props.order.id }} - {{ props.order.createdAt }}
      </p>
    </header>

    <ul>
      <li v-for="item in props.order.items" :key="item.id">
        {{ item.name }} x {{ item.quantity }}
      </li>
    </ul>

    <footer>
      <strong>{{ formatPrice(props.order.total) }}</strong>
      <span v-if="props.order.status === 'prete'" class="ready-pill">Prete</span>
      <button
        v-else
        class="cta-mini"
        type="button"
        @click="emit('ready', props.order.id)"
      >
        Marquer comme prete
      </button>
    </footer>
  </article>
</template>
