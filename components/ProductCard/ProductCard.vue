<template>
  <div class="border border-neutral-200 rounded-md hover:shadow-lg max-w-[300px]">
    <div class="relative">
      <nuxt-link :to="`${product.id}-${titleToUrl}`">
        <img
          v-if="product.image"
          :src="product.image"
          :alt="product.title"
          class="block object-cover h-auto rounded-md aspect-square"
          :width="300"
          :height="300"
        />
      </nuxt-link>
    </div>
    <div class="p-4 border-t border-neutral-200">
      <SfLink href="#" variant="secondary" class="no-underline"> {{ product.title }} </SfLink>
      <div class="flex items-center pt-1">
        <SfRating v-if="product.rating" size="xs" :value="product.rating.rate" :max="5" />

        <SfLink href="#" variant="secondary" class="pl-1 no-underline">
          <SfCounter size="xs">{{ product.rating.count }}</SfCounter>
        </SfLink>
      </div>
      <p class="block py-2 font-normal leading-5 typography-text-sm text-neutral-700">
        {{ product.description.slice(0, 100)}}...
      </p>
      <span class="block pb-2 font-bold typography-text-lg">{{ product.price }}</span>
      <nuxt-link
          :to="`${product.id}-${titleToUrl}`">
          <SfButton type="button" size="sm">
            Read more
          </SfButton>
      </nuxt-link>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { SfRating, SfCounter, SfLink, SfButton } from '@storefront-ui/vue';
import { IProduct } from "~/interface";

const props = defineProps<{
  product: IProduct,
}>()

const titleToUrl = computed(() => props.product.title.toLowerCase().replace(/[^a-z0-9]+/g, '-'))

</script>
