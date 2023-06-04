<template>
  <form ref="referenceRef" role="search">
    <SfInput
      ref="inputRef"
      :value="modelValue"
      @input="emit('update:modelValue', ($event.target as HTMLInputElement).value)"
      aria-label="Search"
      placeholder="Search products"
      @focus="open"
    >
      <template #prefix><SfIconSearch /></template>
      <template #suffix>
        <button
          v-if="modelValue"
          type="button"
          aria-label="Reset search"
          class="flex rounded-md focus-visible:outline focus-visible:outline-offset"
          @click="emit('update:modelValue', '')"
        >
          <SfIconCancel /></button
        ></template>
    </SfInput>
  </form>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {
  SfIconCancel,
  SfIconSearch,
  SfInput,
  useDisclosure,
} from '@storefront-ui/vue';

defineProps<{
  modelValue: string,
}>()

const emit = defineEmits<{
  (event: 'update:modelValue', payload: string): void;
}>();

const inputModel = ref<string>('');
const inputRef = ref<string>();

const { open } = useDisclosure();

const reset = () => {
  inputModel.value = '';
};
</script>
