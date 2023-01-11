<template>
  <div>
    <ol>
      <li
        v-for="product of products"
        :key="product.id"
        :class="{
          highlighted: highlightBrand === product.brand.toLocaleLowerCase(),
        }"
      >
        {{ product.brand }}
      </li>
    </ol>
  </div>
</template>

<script>
export default {
  props: {
    products: {
      type: Array,
      required: true,
    },
    highlightBrand: {
      type: String,
      required: false,
    },
  },

  setup() {
    return {};
  },
};
</script>

<style scoped>
.highlighted {
  color: green;
  font-size: 1.4rem;
}
</style>
