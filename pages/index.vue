<template>
  <div class="page__main">
    <div class="page__main-search">
      <input-search v-model="searchQuery" />
    </div>
    <div class="page__main-list">
      <div v-if="filteredProducts && filteredProducts?.length> 0" class="page__main-item" v-for="product in filteredProducts" :key="product.id">
        <product-card :product="product" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { IProduct } from '~/interface';

useHead({
  title: 'Product Search and Display with SEO',
  meta: [
    {
      name: 'description',
      content: 'A product search and display feature for an e-commerce website while considering SEO best practices'
    },
  ],
});
const searchQuery = ref<string>('');

const { data: products } = await useAsyncData<IProduct[]>('products', () => $fetch('https://fakestoreapi.com/products'))

const filteredProducts = computed<IProduct[] | undefined>(() => {
  //Filtered  products
  if(products?.value){
    return products?.value?.filter(product =>
    product.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
  }
});

watch<string>(searchQuery, () => {
  // Update the URL with search query parameter
  const currentPath = window.location.pathname;
  const urlSearchParams = new URLSearchParams(window.location.search);
  urlSearchParams.set('search', searchQuery.value);
  const newURL = `${currentPath}?${urlSearchParams.toString()}`;
  window.history.replaceState({}, '', newURL);
})
</script>

<style lang="scss">
.page__main{
  max-width: 1200px;
  margin: 0 auto;
  padding: 60px 0;
  &-list{
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 30px;
    margin: 30px 0;
  }
}
</style>
