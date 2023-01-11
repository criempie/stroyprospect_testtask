<template>
  <div class="container">
    <label>
      Запрос
      <input v-model.trim="searchName" />
    </label>
    <label>
      Мой бренд
      <input v-model.trim="highlightBrand" />
    </label>
    <button @click="getProducts">Поиск</button>
  </div>

  <list-component
    :products="products"
    :highlightBrand="highlightBrandLowerCase"
  ></list-component>
</template>

<script>
import listComponent from "./components/list-component.vue";
import axios from "axios";
import { computed, ref } from "vue";

export default {
  name: "App",
  components: {
    listComponent,
  },
  setup() {
    const apiUrl = "https://search.wb.ru/exactmatch/ru/common/v4/search";
    const queryParams = {
      appType: 1,
      curr: "rub",
      dest: "-1113276,-79379,-1104258,-5803327",
      lang: "ru",
      locale: "ru",
      pricemarginCoeff: "1.0",
      reg: 0,
      resultset: "catalog",
      sort: "popular",
    };

    const products = ref([]);

    let searchName = ref("");
    let highlightBrand = ref("");
    const highlightBrandLowerCase = computed(() => {
      return highlightBrand.value.toLocaleLowerCase();
    });

    function getProducts() {
      const url = new URL(apiUrl);

      for (const [name, value] of Object.entries(queryParams)) {
        url.searchParams.append(name, value);
      }

      url.searchParams.append("query", searchName.value);

      axios.get(url.href).then((res) => {
        products.value = res.data?.data?.products ?? [];
      });
    }

    return {
      products,
      getProducts,
      highlightBrand,
      searchName,
      highlightBrandLowerCase,
    };
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
}
</style>
