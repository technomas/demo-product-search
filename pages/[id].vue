<template>
  <div class="page__product">
    <nuxt-link
      :to="{
        name: 'index'
      }"
      class="leading-5 no-underline text-inherit hover:underline active:underline whitespace-nowrap outline-secondary-600"
    >
      Back
    </nuxt-link>
    <product-card-full :product="product" />
  </div>
</template>

<script setup>
const route = useRoute();

const { data: product } = {} = await useAsyncData('product', () => $fetch(`https://fakestoreapi.com/products/${route.params.id}`))

useSeoMeta({
  title: () => product.value?.title,
  ogTitle: () => product.value?.title,
  description: () => product.value?.description,
  ogDescription: () => product.value?.description,
  ogImage: () => product.value?.image,
  twitterCard: 'summary_large_image',
})
</script>

<style lang="scss">
.page__product{
  max-width: 1024px;
  margin: 30px auto;
}
</style>
