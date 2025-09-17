<template>
  <div class="mx-auto">
    <LandingPageHeader 
      subtitle="<a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/automaten'>Pringles<span class='text-lg align-super'>®</span> Automaten</a> kaufen - direkt vom Pringles<span class='text-lg align-super'>®</span> Hersteller"
      description="<a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/automaten'>Pringles<span class='text-sm align-super'>®</span> Automaten</a> – Snacklösung zum Aufstellen<br>
                  Einfach aufstellen – fertig.<br>
                  Der Pringles<span class='text-sm align-super'>®</span> Drehautomat sorgt für Sichtbarkeit im Raum und bringt Snacks dahin, wo sie gefragt sind. Er kommt in Signalrot oder Schwarz, wird mit 49 Dosen à 40 g bestückt und ist direkt einsatzbereit – ohne Strom, ohne Technik. Alternativ ist der Automat auch in der Thekenvariante mit 28 à 40 g verfügbar, Ob im Kiosk, an der Tankstelle oder beim Event – einmal gedreht, schon gekauft. Und wenn’s ohne Münzeinwurf sein soll: Der passende Dispenser macht’s möglich – für Theken, Self-Service oder den Kassenbereich."
      noMarginTop
      headerTop
    />

    <LandingPagePresentation imageSrc="/images/snackpringles/automaten.png" 
        Title="Wenn sich was dreht – ganz ohne Technik" 
        Description="Der Drehautomat funktioniert mechanisch – kein Stromanschluss, keine Wartung. Kund:innen verstehen das System auf einen Blick: Geld einwerfen, drehen, fertig. Die Dosen lassen sich schnell auffüllen, das Handling ist einfach.
                  Mit dem optionalen Dispenser kannst du die Produkte zusätzlich platzieren – da, wo sie direkt ins Auge fallen."
        noButton
        noPadding
        />
    <LandingPagePresentation imageLeft :bgColor="'bg-gray-100'" imageSrc="/images/snackpringles/2.jpg" 
        Title="Marke zeigen – Aufmerksamkeit nutzen" 
        Description="Pringles<span class='text-sm align-super'>®</span> ist bekannt* – das erleichtert den Verkauf. Die auffällige Optik macht den Automaten zum Hingucker, das vertraute Produkt zum Impulsgeber. Drehautomat und Dispenser – beide brauchen wenig Platz und können flexibel integriert werden. So wird aus einem freien Spot schnell ein Snackangebot, das sich sehen lassen kann." 
        noButton
        noPadding
        />
    

    <LandingPagePresentation imageSrc="/images/snackpringles/3.jpg" 
      Title="Praktisch im Handling, klar im Konzept" 
      Description="Aufstellen, nachfüllen, verkaufen – mehr braucht’s nicht. Der Automat funktioniert ohne Technik, der Dispenser ohne Münzeinwurf. Beides lässt sich gut kombinieren – zum Beispiel Automaten fürs Snackregal, Dispenser für die Theke. Die Nachbestellung der Dosen? Geht direkt über uns.<br>
                  Jetzt Automatenlösungen entdecken – und dein Sortiment gezielt ergänzen."
      Additional="* Quelle: NIQ Brand Health Tracker 2024"
      noButton
      noPadding
      />

  </div>
</template>
<script lang="ts" setup>

const { data: productsCatalog, loading, fetchProducts } = useProducts();
const { isAuthorized } = useCustomer();
import { SfLoaderCircular } from '@storefront-ui/vue';
import { Product } from "@plentymarkets/shop-api";
import { useAsyncData } from 'nuxt/app';

const pringlesProducts = ref([] as Product[]);
const cerealienProducts = ref([] as Product[]);

const { data: pringlesData } = await useAsyncData('pringlesProducts', () => fetchProducts({ categoryUrlPath: 'snacks/pringles', page: 1, itemsPerPage: 4 }))
const { data: cerealienData } = await useAsyncData('cerealienProducts', () => fetchProducts({ categoryUrlPath: 'fruehstueck/cerealien', page: 1, itemsPerPage: 4 }))

// Set initial values from SSR/CSR fetch
if (pringlesData.value?.products) pringlesProducts.value = pringlesData.value.products;
if (cerealienData.value?.products) cerealienProducts.value = cerealienData.value.products;

async function reloadProducts() {
  const pringlesRes  = await fetchProducts({ categoryUrlPath: 'snacks/pringles', page: 1, itemsPerPage: 4 })
  const cerealienRes = await fetchProducts({ categoryUrlPath: 'fruehstueck/cerealien', page: 1, itemsPerPage: 4 })
  pringlesProducts.value  = pringlesRes.products
  cerealienProducts.value = cerealienRes.products 
}

watch(isAuthorized, (newVal) => {
  if (newVal) {
    reloadProducts()
  }
})

const { getRobots, setRobotForStaticPage } = useRobots();


await getRobots();
setRobotForStaticPage('Homepage');

</script>



<style>
.klg-red {
  border-color: #f60b45;
  color: #f60b45;
}

.global-sizes { 
  max-width: 1200px;
  min-height: 355px;
}
</style>