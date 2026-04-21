<script setup>
const props = defineProps({
  dish: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['festoyons'])

const formatPrice = (price) => {
  return new Intl.NumberFormat('fr-FR', {
    style: 'currency',
    currency: 'EUR',
  }).format(price)
}
</script>

<template>
  <article class="gourmet-card">
    <div class="gourmet-card__top">
      <h3>{{ props.dish.name }}</h3>
      <span id="victuaille" class="victuaille-badge">
        <slot name="badge">
          <span class="chip chip--classic">Signature</span>
        </slot>
      </span>
    </div>

    <slot name="details">
      <p class="dish-description">{{ props.dish.description }}</p>
    </slot>

    <p class="dish-meta">
      {{ props.dish.category }} | {{ props.dish.prepTime }} min
    </p>

    <div class="gourmet-card__bottom">
      <strong>{{ formatPrice(props.dish.price) }}</strong>
      <button class="cta-mini" type="button" @click="emit('festoyons', props.dish)">
        Ajouter au panier
      </button>
    </div>
  </article>
</template>
