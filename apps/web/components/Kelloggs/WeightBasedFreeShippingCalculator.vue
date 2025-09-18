<template>
  <div v-if="isAuthorized && userClassId === 2" class="kl-shipping-progress pb-2">
    <div class="flex rounded-[1vw] overflow-hidden w-full text-sm font-medium text-white">
      <!-- Text Section -->
      <div class="bg-[#2ea533] flex-1 px-4 py-2 flex items-center">
        <p class="!text-white shipping-text m-0">
          <template v-if="remainingWeight > 0">
            Es fehlen noch
            <strong class="font-bold text-white">
              {{ formattedRemainingWeight }} kg
            </strong>
            , um bestellen zu können
          </template>
          <template v-else>
            Gewicht ist in Ordnung, Sie können zur Kasse gehen
          </template>
        </p>
      </div>

      <!-- Arrow Section -->
      <div class="bg-[#1f6522] px-4 flex items-center justify-center">
        <svg
          class="w-4 h-4"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          viewBox="0 0 24 24"
        >
          <path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7" />
        </svg>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import { useI18n } from 'vue-i18n';
import type { Product } from '@plentymarkets/shop-api';

const props = defineProps<{
  cart: any;
}>();

const { data: userData, isAuthorized } = useCustomer();
const { n } = useI18n();

const userClassId = computed(() => userData?.value?.user?.classId);
const FREE_SHIPPING_WEIGHT_THRESHOLD = 50; // 50 kg

const getWeight = (product: Product) => {
  let propertyGroup = product.variationProperties?.find(property => property.id == 8);
  let property: any = propertyGroup?.properties.find(property => property.id == 21);
  if (!property) return 0;

  return property.values.value;
}

const cartTotalWeight = ref(50);

watch(() => props.cart?.items, async (items) => {
  if (!items) return;

  let totalWeight = 0;
  for (const item of items) {
    const { data: product, fetchProduct } = useProduct(item.variation.item.id.toString());
    await fetchProduct({ id: item.variation.item.id });
    const weight = getWeight(product.value);
    totalWeight += weight * item.quantity;
  }
  cartTotalWeight.value = totalWeight / 1000; // convert grams to kilograms
}, { immediate: true, deep: true });


const remainingWeight = computed(() => Math.max(0, FREE_SHIPPING_WEIGHT_THRESHOLD - cartTotalWeight.value));
const formattedRemainingWeight = computed(() => n(remainingWeight.value, { minimumFractionDigits: 2, maximumFractionDigits: 2 }));

const emit = defineEmits(['update:remainingWeight']);

watch(remainingWeight, (newValue) => {
  emit('update:remainingWeight', newValue);
}, { immediate: true });
</script>
