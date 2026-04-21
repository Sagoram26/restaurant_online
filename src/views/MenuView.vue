<script setup>
import { inject, ref, watch } from 'vue'
import Gourmet from '../components/Gourmet.vue'
import { menuData } from '../data/menu'

const store = inject('restaurantStore')
const dishes = ref(menuData)
const lastAddedDish = ref(null)

const onDishAdded = (dish) => {
  store.addDishToCart(dish)
  lastAddedDish.value = { ...dish, addedAt: Date.now() }
}

watch(lastAddedDish, (dish) => {
  if (!dish) {
    return
  }
  store.showToast(`${dish.name} ajoute au panier.`)
})
</script>

<template>
  <section class="view-shell">
    <div class="view-title">
      <h2>Notre Menu</h2>
      <p>Choisis tes plats puis clique sur Ajouter au panier.</p>
    </div>

    <div class="menu-grid">
      <Gourmet v-for="dish in dishes" :key="dish.id" :dish="dish" @festoyons="onDishAdded">
        <template #badge>
          <span v-if="dish.isNew" class="chip chip--new">Nouveau</span>
          <span v-else class="chip chip--classic">Le classique</span>
        </template>

        <template #details>
          <p class="dish-description">{{ dish.description }}</p>
        </template>
      </Gourmet>
    </div>
  </section>
</template>
