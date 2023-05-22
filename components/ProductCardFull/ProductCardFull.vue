<template>
  <section class="md:max-w-[640px]">
    <img
      :src="product.image"
      :alt="product.title"
      class="block object-cover h-auto rounded-md aspect-square m-auto mb-2"
      :width="300"
      :height="300"
    />
    <h1 class="mb-1 font-bold typography-headline-4">{{ product.title }}</h1>
    <strong class="block font-bold typography-headline-3">{{ product.price }}</strong>
    <div class="inline-flex items-center mt-4 mb-2">
      <SfRating size="xs" :value="product.rating.rate" :max="5" />
      <SfCounter class="ml-1" size="xs">{{ product.rating.count }}</SfCounter>
    </div>
    <p class="mb-4 font-normal typography-text-sm">
      {{ product.description }}
    </p>
    <div class="py-4 mb-4 border-gray-200 border-y">
      <div
          class="bg-primary-100 text-primary-700 flex justify-center gap-1.5 py-1.5 typography-text-sm items-center mb-4 rounded-md"
      >
        <SfIconShoppingCartCheckout />
        1 in cart
      </div>
      <div class="items-start xs:flex">
        <div class="flex flex-col items-stretch xs:items-center xs:inline-flex">
          <div class="flex border border-neutral-300 rounded-md">
            <SfButton
                type="button"
                variant="tertiary"
                :disabled="count <= min"
                square
                class="rounded-r-none p-3"
                :aria-controls="inputId"
                aria-label="Decrease value"
                @click="dec()"
            >
              <SfIconRemove />
            </SfButton>
            <input
                :id="inputId"
                v-model="count"
                type="number"
                role="spinbutton"
                class="grow appearance-none mx-2 w-8 text-center bg-transparent font-medium [&::-webkit-inner-spin-button]:appearance-none [&::-webkit-inner-spin-button]:display-none [&::-webkit-inner-spin-button]:m-0 [&::-webkit-outer-spin-button]:display-none [&::-webkit-outer-spin-button]:m-0 [-moz-appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none disabled:placeholder-disabled-900 focus-visible:outline focus-visible:outline-offset focus-visible:rounded-sm"
                :min="min"
                :max="max"
                @input="handleOnChange"
            />
            <SfButton
                type="button"
                variant="tertiary"
                :disabled="count >= max"
                square
                class="rounded-l-none p-3"
                :aria-controls="inputId"
                aria-label="Increase value"
                @click="inc()"
            >
              <SfIconAdd />
            </SfButton>
          </div>
          <p class="self-center mt-1 mb-4 text-xs text-neutral-500 xs:mb-0">
            <strong class="text-neutral-900">{{ max }}</strong> in stock
          </p>
        </div>
        <SfButton type="button" size="lg" class="w-full xs:ml-4">
          <template #prefix>
            <SfIconShoppingCart size="sm" />
          </template>
          Add to cart
        </SfButton>
      </div>
      <div class="flex justify-center mt-4 gap-x-4">
        <SfButton type="button" size="sm" variant="tertiary">
          <template #prefix>
            <SfIconCompareArrows size="sm" />
          </template>
          Compare
        </SfButton>
        <SfButton type="button" size="sm" variant="tertiary">
          <SfIconFavorite size="sm" />
          Add to list
        </SfButton>
      </div>
    </div>
  </section>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {
    SfButton,
    SfCounter,
    SfLink,
    SfRating,
    SfIconSafetyCheck,
    SfIconCompareArrows,
    SfIconWarehouse,
    SfIconPackage,
    SfIconFavorite,
    SfIconSell,
    SfIconShoppingCart,
    SfIconAdd,
    SfIconRemove,
    useId,
    SfIconShoppingCartCheckout,
} from '@storefront-ui/vue';
import { clamp } from '@storefront-ui/shared';
import { useCounter } from '@vueuse/core';
import {IProduct} from "~/interface";

defineProps<{
	product: IProduct,
}>()

const inputId = useId();
const min = ref(1);
const max = ref(999);
const { count, inc, dec, set } = useCounter(1, { min: min.value, max: max.value });
function handleOnChange(event: Event) {
    const currentValue = (event.target as HTMLInputElement)?.value;
    const nextValue = parseFloat(currentValue);
    set(clamp(nextValue, min.value, max.value));
}
</script>
