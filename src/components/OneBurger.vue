<script>
export default {
  name: 'OneBurger',
  components: {
  },
  props: {
    burger: {
      type: Object,
      required: true
    }
  },
  methods: {

    AddBurger: function (burger) {
      burger.amountOrdered += 1;
    },
    RemoveBurger: function (burger) {
      if (burger.amountOrdered > 0) {
        burger.amountOrdered -= 1;
      }
    }
  },
}
</script>

<template>
  <article class="one-burger">
    <img class="burger-image" :src="burger.url" />

    <div class="burger-info">
      <h4 class="burger-name">{{ burger.name }}</h4>
      <div class="burger-kCal">kCal: {{ burger.kCal ?? burger.kcal ?? '—' }}</div>

      <div class="burger-allergy">
        <span v-if="burger.gluten" class="badge">Contains gluten</span>
        <span v-else class="badge muted">Gluten-free</span>

        <span v-if="burger.lactose" class="badge">Contains lactose</span>
        <span v-else class="badge muted">Lactose-free</span>
      </div>
    </div>
    <div>
      <button v-on:click="RemoveBurger(burger)">-</button>
      <button v-on:click="AddBurger(burger)">+</button>
      {{ burger.amountOrdered }}
    </div>
  </article>
</template>

<style scoped>
.one-burger {
  display: flex;
  flex-direction: column;
  border-radius: 8px;
  background: #fff;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.06);
  overflow: hidden;
}

.burger-image {
  width: 100%;
  height: 160px;
  object-fit: cover;
  display: block
}

.burger-info {
  padding: 0.75rem;
}

.burger-name {
  margin: 0 0 0.25rem 0;
  font-size: 1.05rem
}

.burger-kCal {
  color: #666;
  font-size: .9rem
}

.burger-allergy {
  margin-top: 0.5rem;
  display: flex;
  gap: .5rem;
  flex-wrap: wrap
}

.badge {
  background: #ffefeb;
  color: #b0421f;
  padding: .25rem .5rem;
  border-radius: 4px;
  font-size: .75rem
}

.badge.muted {
  background: #f3f3f3;
  color: #666
}

.selected-burger {
  border: 5px solid red;
}
</style>