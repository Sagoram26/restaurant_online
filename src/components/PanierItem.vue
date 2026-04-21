<script setup>
const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['update-quantity', 'remove'])

const formatPrice = (price) => {
  return new Intl.NumberFormat('fr-FR', {
    style: 'currency',
    currency: 'EUR',
  }).format(price)
}

const changeQuantity = (nextValue) => {
  const safeValue = Math.max(1, Number(nextValue) || 1)
  emit('update-quantity', { id: props.item.id, quantity: safeValue })
}

const handleManualInput = (event) => {
  changeQuantity(event.target.value)
}
</script>

<template>
  <article class="panier-item">
    <div>
      <h3>{{ props.item.name }}</h3>
      <p>{{ formatPrice(props.item.price) }} / unite</p>
    </div>

    <div class="panier-item__controls">
      <button type="button" @click="changeQuantity(props.item.quantity - 1)">-</button>
      <input
        type="number"
        min="1"
        :value="props.item.quantity"
        @change="handleManualInput"
      />
      <button type="button" @click="changeQuantity(props.item.quantity + 1)">+</button>
    </div>

    <p class="panier-item__subtotal">
      {{ formatPrice(props.item.price * props.item.quantity) }}
    </p>

    <button class="ghost-danger" type="button" @click="emit('remove', props.item.id)">
      Supprimer
    </button>
  </article>
</template>
