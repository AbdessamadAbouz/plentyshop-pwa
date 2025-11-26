<template>
  <UiOverlay :visible="open">
    <SfDrawer
      ref="productLegalDrawerRef"
      v-model="open"
      data-testid="product-legal-details-drawer"
      :placement="placement"
      :class="[
        'lg:w-128',
        'bg-neutral-50',
        'border',
        'border-gray-300',
        'z-50',
        { 'lg:min-w-[400px]': placement === 'left' || placement === 'right' },
      ]"
    >
      <header class="flex items-center justify-between px-10 py-6 bg-primary-500">
        <div class="flex items-center text-white">{{ t('productLegalDetailsHeader') }}</div>
        <UiButton
          square
          variant="tertiary"
          data-testid="product-legal-details-close"
          class="text-white"
          @click="open = false"
        >
          <SfIconClose />
        </UiButton>
      </header>

      <div
        ref="tablistRef"
        role="tablist"
        aria-label="Select tab"
        aria-orientation="horizontal"
        class="flex gap-2 border-b border-b-neutral-200 p-4 overflow-x-auto [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]"
      >
        <UiButton
          v-for="(tab, index) in tabs"
          :key="tab.label"
          type="button"
          role="tab"
          :data-testid="tab.component.__name"
          :variant="isActiveTab(index) ? 'primary' : 'secondary'"
          :aria-selected="isActiveTab(index)"
          :aria-controls="`tabpanel-${index}`"
          :disabled="tab.disabled"
          @click="setActiveTab(index)"
        >
          {{ tab.label }}
        </UiButton>
      </div>
      <div
        v-for="(tab, index) in tabs"
        v-show="isActiveTab(index)"
        :id="`tabpanel-${index}`"
        :key="tab.label"
        role="tabpanel"
        :aria-labelledby="`tab-${index}`"
        class="p-4"
      >
        <component :is="tab.component" :product="props.product" />
      </div>
    </SfDrawer>
  </UiOverlay>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import type { SfDrawerPlacement } from '@storefront-ui/vue';
import { SfDrawer, SfIconClose, useTrapFocus } from '@storefront-ui/vue';
import { manufacturerGetters, productGetters } from '@plentymarkets/shop-api';
import type { ProductLegalDetailsProps } from '~/components/ProductLegalDetailsDrawer/types';
import ManufacturerResponsibleInfo from '~/components/ManufacturerResponsibleInfo/ManufacturerResponsibleInfo.vue';
import ManufacturerInformation from '~/components/ManufacturerInformation/ManufacturerInformation.vue';

const props = defineProps<ProductLegalDetailsProps>();

const { t } = useI18n();

const placement = ref<`${SfDrawerPlacement}`>('right');

const manufacturer = computed(() => productGetters.getManufacturer(props.product));

const responsibleCountry = computed(() => manufacturerGetters.getManufacturerResponsibleCountry(manufacturer.value));
const responsibleInfo = computed(() => ({
  name: manufacturerGetters.getManufacturerResponsibleName(manufacturer.value),
  street: manufacturerGetters.getManufacturerResponsibleStreet(manufacturer.value),
  houseNo: manufacturerGetters.getManufacturerResponsibleHouseNo(manufacturer.value),
  postCode: manufacturerGetters.getManufacturerResponsiblePostCode(manufacturer.value),
  town: manufacturerGetters.getManufacturerResponsibleTown(manufacturer.value),
  country: Object.keys(responsibleCountry.value || {}).length > 0 ? responsibleCountry.value : null,
  email: manufacturerGetters.getManufacturerResponsibleEmail(manufacturer.value),
  phoneNo: manufacturerGetters.getManufacturerResponsiblePhoneNo(manufacturer.value),
  responsibleContactUrl: manufacturerGetters.getManufacturerResponsibleContactUrl(manufacturer.value),
}));

const manufacturerCountry = computed(() => manufacturerGetters.getManufacturerCountry(manufacturer.value));
const manufacturerInfo = computed(() => ({
  logo: manufacturerGetters.getManufacturerLogo(manufacturer.value),
  name: manufacturerGetters.getManufacturerName(manufacturer.value),
  externalName: manufacturerGetters.getManufacturerExternalName(manufacturer.value),
  legalName: manufacturerGetters.getManufacturerLegalName(manufacturer.value),
  street: manufacturerGetters.getManufacturerStreet(manufacturer.value),
  houseNo: manufacturerGetters.getManufacturerHouseNo(manufacturer.value),
  postcode: manufacturerGetters.getManufacturerPostCode(manufacturer.value),
  town: manufacturerGetters.getManufacturerTown(manufacturer.value),
  country: Object.keys(manufacturerCountry.value || {}).length > 0 ? manufacturerCountry.value : null,
  email: manufacturerGetters.getManufacturerEmail(manufacturer.value),
  phoneNumber: manufacturerGetters.getManufacturerPhoneNumber(manufacturer.value),
  faxNumber: manufacturerGetters.getManufacturerFaxNumber(manufacturer.value),
  contactUrl: manufacturerGetters.getManufacturerContactUrl(manufacturer.value),
  url: manufacturerGetters.getManufacturerUrl(manufacturer.value),
}));

const hasResponsibleInfo = computed(() => Object.values(responsibleInfo.value).some(Boolean));
const hasManufacturerInfo = computed(() => Object.values(manufacturerInfo.value).some(Boolean));

const tabs = computed(() => {
  if (hasResponsibleInfo.value || hasManufacturerInfo.value) {
    return [
      {
        label: t('manufacturer.combinedTabName'),
        component: hasResponsibleInfo.value ? ManufacturerResponsibleInfo : ManufacturerInformation,
        disabled: false,
      },
    ];
  }
  return [];
});

const activeTabIndex = ref(0);

watch(
  () => tabs.value.length,
  (len) => {
    if (len === 0) {
      activeTabIndex.value = -1;
    } else if (activeTabIndex.value >= len) {
      activeTabIndex.value = 0;
    }
  },
  { immediate: true },
);

const isActiveTab = (index: number) => activeTabIndex.value === index && tabs.value.length > 0;
const setActiveTab = (index: number) => {
  activeTabIndex.value = index;
};

const productLegalDrawerRef = ref();
const { open } = useProductLegalDetailsDrawer();
useTrapFocus(productLegalDrawerRef, { activeState: open });
</script>
